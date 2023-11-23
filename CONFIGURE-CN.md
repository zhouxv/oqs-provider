# 配置 oqsprovider

本文档列出了`oqsprovider`的所有配置选项。

## 预构建配置

此代码的重要部分由脚本 `oqs-template/generate.py` 生成。该脚本允许集成和激活所有由 [liboqs](https://github.com/open-quantum-safe/liboqs) 提供的量子安全算法。默认配置如 [算法配置文件](oqs-template/generate.yml) 中所定义。

任何更改此默认值的PR必须包含 `generate.py` 更改的所有文件。

## 构建安装选项

### OPENSSL_ROOT_DIR

通过 `cmake` 定义，为 OpenSSL(v3) 安装指定非标准位置。默认情况下，此值未设置，要求在标准 OS 部署位置中存在 OpenSSL3 安装。

### CMAKE_BUILD_TYPE

将此 `cmake` 配置选项设置为 "Release" 会禁用所有调试输出。这是默认设置。

在出现任何问题的情况下，强烈建议将此值设置为 "Debug" 以激活更多的警告消息。特别是在设置为 "Debug" 时，会激活不同的 [调试功能](https://github.com/open-quantum-safe/oqs-provider/wiki/Debugging) 并输出额外的设置警告。

### liboqs_DIR

此环境变量必须设置为要在构建中使用的 `liboqs` 安装的位置。默认情况下，此变量未设置，要求将 `liboqs` 安装在 OS 的标准位置。这使用 `cmake` 中的 [`find_package`](https://cmake.org/cmake/help/latest/command/find_package.html) 命令，该命令检查位于 `<PackageName>_DIR` 处的包的本地构建。

### USE_ENCODING_LIB

通过在编译时设置 `-DUSE_ENCODING_LIB=<ON/OFF>`，可以使用外部编码库 `qsc-key-encoder` 编译 `oqs-provider`。通过环境配置编码，详见 [ALGORITHMS.md](ALGORITHMS.md)。默认值为 `OFF`。

### NOPUBKEY_IN_PRIVKEY

将其设置为 "ON"，可以指定在 `privateKey` 结构中显式序列化公钥，例如，用于互操作性测试。默认值为 `OFF`。

### OQS_KEM_ENCODERS

将其设置为 "ON"，配置 `oqsprovider` 为 KEM 算法提供公钥和私钥文件格式的编码器和解码器。这会增加提供程序的大小，但启用更多用例。默认值为 `OFF`。

### OQS_PROVIDER_BUILD_STATIC

在编译时设置 `-DOQS_PROVIDER_BUILD_STATIC=ON`，可以将 `oqs-provider` 编译为静态库（`oqs-provider.a`）。当作为静态库构建时，提供程序入口点的名称为 `oqs_provider_init`。可以使用 [`OSSL_PROVIDER_add_builtin`](https://www.openssl.org/docs/man3.1/man3/OSSL_PROVIDER_add_builtin.html) 函数添加提供程序：

```c
#include <openssl/provider.h>

// 入口点。
extern OSSL_provider_init_fn oqs_provider_init;

void load_oqs_provider(OSSL_LIB_CTX *libctx) {
  int err;

  if (OSSL_PROVIDER_add_builtin(libctx, "oqsprovider", oqs_provider_init) == 1) {
    if (OSSL_PROVIDER_load(libctx, "oqsprovider") == 1) {
      fputs("successfully loaded `oqsprovider`.", stderr);
    } else {
      fputs("failed to load `oqsprovider`", stderr);
    }
  } else {
    fputs("failed to add the builtin provider `oqsprovider`", stderr);
  }
}
```

> **警告**
> `OQS_PROVIDER_BUILD_STATIC` 和 `BUILD_SHARED_LIBS` 是互斥的。

## 便捷构建脚本选项

对于任何希望构建完整软件栈（`openssl`(v3)、`liboqs` 和 `oqs-provider`）的人来说，`scripts` 目录中的 `fullbuild.sh` 和 `runtests.sh` 文件适用于此。这些文件可以通过以下环境变量进行配置：

### OPENSSL_INSTALL

现有非标准 OpenSSL 二进制发行版的目录。

### OPENSSL_BRANCH

要构建和在测试中使用的特定 OpenSSL 发布的标签。由于许多已知与提供程序相关的代码缺陷，谨慎设置为低于 "3.0.9" 的值。

### LIBOQS_BRANCH

定义了 `oqs-provider` 构建的 `liboqs` 分支。例如，可以使用此选项为 `oqsprovider` 发布跟踪旧/稳定的 `liboqs` 发布。如果未设置此变量，则构建 "main" 分支。

### liboqs_DIR

如果设置了此环境变量，则不会构建 `liboqs`，而是从此变量中指定的目录中使用。该位置必须包含 `include` 和 `lib` 目录。未设置此变量，将从源代码构建 `liboqs`。

### MAKE_PARAMS

此环境变量允许将参数传递给用于构建 `openssl` 的 `make` 命令，例如 "-j 8"，以在适用的多核机器上减少编译时间的并行构建。

### OQSPROV_CMAKE_PARAMS

此环境变量允许将参数传递给用于构建 `oqsprovider` 的 `cmake` 命令。

### OQS_SKIP_TESTS

通过设置此测试环境变量，可以禁用测试中列出的特定算法家族的测试。默认情况下，此变量未设置。

示例用法：

    OQS_SKIP_TESTS="sphincs" ./scripts/runtests.sh

排除所有 "Sphincs" 家族的算法（显著加快测试速度）。

*注意*：默认情况下，不再执行与 oqs-openssl111 的互操作性测试，但可以在

脚本 `scripts/runtests.sh` 中手动启用。

### OPENSSL_CONF

此测试环境变量可用于指示 `openssl` 使用非标准位置的配置文件。设置此值还会禁用内置于 `runtests.sh` 中的自动化逻辑，因此在设置时需要了解 `openssl` 操作。默认情况下，此变量未设置。

## LIBOQS 配置选项

[在此处完整记录了这些选项](https://github.com/open-quantum-safe/liboqs/wiki/Customizing-liboqs)。这里特别上下文的一个选项是，特别是如果构建 `oqs-provider` 为静态库，即作为不需要在部署期间存在 `liboqs` 的独立二进制文件：

### OQS_ALGS_ENABLED

为了减小 oqsprovider 的大小，可以通过设置 `liboqs` 构建选项 `-DOQS_ALGS_ENABLED=STD` 来限制支持的算法数量，例如，仅支持 NIST 标准化的算法集。`oqs-provider` 支持的算法列表由文件 `generate.yml` 的内容定义，该文件在 [预构建配置](#pre-build-configuration) 中进行了记录。

## 运行时选项

可以利用 `openssl` [属性选择机制](https://www.openssl.org/docs/manmaster/man7/property.html) 来进行运行时算法选择。

### oqsprovider.security_bits

可以通过在密钥 "oqsprovider.security_bits" 上使用 openssl 属性选择机制来选择特定位强度的算法，例如，`openssl list -kem-algorithms -propquery oqsprovider.security_bits=256`。混合算法的位强度始终由经典算法的位强度定义。