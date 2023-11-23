# 开发指南

## 基础

每个开发者都有自己的编码风格，总的来说，多样性是好的，也是受欢迎的。

尽管如此，在这个项目中，我们仍然试图遵循一些基本的目标：

- 所有部分都应该是可读/可理解的，而不必首先理解所有部分。
- 因此，鼓励使用注释（包括在合适的地方进行交叉引用）。
- 为了语法上的可读性，项目采用了 [OpenSSL 编码规范](https://www.openssl.org/policies/technical/coding-style.html)。
- 存在用于验证编码规范的工具：只需执行 `clang-format --dry-run --Werror file-to-test`。

- 应尽量避免平台特定的代码，因为该项目旨在至少在 Linux、MacOS 和 Windows（x64 和 aarch64 架构）上正确运行。

## 生成的代码

代码的重要部分是通过脚本 `oqs-template/generate.py` 生成的。
此脚本用于将 [liboqs](https://github.com/open-quantum-safe/liboqs) 的特定版本导入到 `oqsprovider` 中。
特别是控制文件 `oqs-template/generate.yml` 必须与特定的 `liboqs` 版本同步：算法 ID，例如，签名算法 OID 必须与特定算法代码版本对齐。
因此，在生成器括号内的代码不得更改：

```
///// OQS_TEMPLATE_FRAGMENT_..._START
...
///// OQS_TEMPLATE_FRAGMENT_..._END
```

如果需要更改这些代码，则必须在 `oqs-template` 目录中的生成器代码片段中实现。

在正常的代码开发过程中，很少有必要触及这些文件中的任何一个。

## 普通构建

如果在开发机器上满足 `oqsprovider` 的先决条件，即存在 `liboqs` 和 `openssl`（v.3），则可以通过运行 `scripts/fullbuild.sh` 来执行构建。脚本中有各种参数，并在 [文档中](CONFIGURE.md#convenience-build-script-options) 中记录，以适应特定的构建环境。该脚本还可以用于构建特定版本的 `openssl` 和 `liboqs`，以及所有组件的调试版本。

## 普通测试

所有用于本地功能测试的测试都集成/可用于在脚本 `scripts/runtest.sh` 中执行。只有当所有测试在本地通过时，才应考虑合并请求，因为CI系统也使用这些测试。

## 调试

项目特定的调试工具在 [wiki](https://github.com/open-quantum-safe/oqs-provider/wiki/Debugging) 中有文档记录。

对于 "传统" 的 `gdb` 风格调试，请确保在构建 `oqsprovider` 时设置 "-DCMAKE_BUILD_TYPE=Debug"，并在配置 `openssl` 时设置 "-d"（有关在何处最好执行此操作，请参见 "scripts/fullbuild.sh" 中的其他信息）。
