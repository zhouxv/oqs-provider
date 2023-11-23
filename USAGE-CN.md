`oqsprovider`使用说明
=======================

此文件记录了在运行`openssl` v3的机器上安装`oqsprovider`后，正确使用它所需的信息。

请注意，`oqsprovider`将无法在安装了低于“3.0.0”版本的OpenSSL的机器上运行（这是默认安装的版本）。

## 基本假设

假定已经安装了`openssl`版本>= 3.0.0，并且已在"PATH"环境变量中设置了该版本，以便运行`openssl version`命令产生如下结果：

```bash
OpenSSL 3.2.0-dev  (Library: OpenSSL 3.2.0-dev )
```

## 激活

每个OpenSSL提供者在使用之前都需要激活。有三种主要方法：

### 显式命令行选项

#### -provider

大多数`openssl`命令都允许传递`-provider`选项：该命令后面的名称是要激活的提供者的名称。

例如：`openssl list -signature-algorithms -provider oqsprovider` 将输出所有供`openssl`使用的量子安全签名算法。

#### -provider-path

所有接受`-provider`的`openssl`命令也允许传递`-provider-path`，作为引用本地文件系统中提供者二进制文件位置的可能性。如果提供者尚未安装在系统位置中，这将特别有用，通常该位置在`lib/ossl-modules`中，位于主`openssl`安装树中。

### C API

如果要在C程序中硬编码激活`oqsprovider`，可以考虑使用标准的 [OSSL_PROVIDER_load](https://www.openssl.org/docs/man3.1/man3/OSSL_PROVIDER_load.html) API，例如：

```c
OSSL_PROVIDER_load(OSSL_LIB_CTX_new(), "oqsprovider");
```

提供者（二进制文件）的搜索路径可以通过 [OSSL_PROVIDER_set_default_search_path](https://www.openssl.org/docs/manmaster/man3/OSSL_PROVIDER_set_default_search_path.html) API 设置。

#### 静态链接

如果希望将所有代码“嵌入”到一个可执行文件中，请确保使用`OQS_PROVIDER_BUILD_STATIC`配置选项静态构建`oqsprovider`，并使用[文档中记录的集成代码](CONFIGURE.md#oqs_provider_build_static)。

### 配置文件

作为传递命令行参数或硬编码C代码的替代方法，可以通过向`openssl.cnf`文件添加指令来激活提供者以供一般使用。对于`oqs-provider`，请添加以下行：

```ini
[provider_sect]
default = default_sect
oqsprovider = oqsprovider_sect
[oqsprovider_sect]
activate = 1
```

#### module 选项

除了"activate"关键字之外，`openssl`还识别"module"关键字，它反映了上面记录的`-provider-path`的功能：这种方式可以注册用于测试的`oqsprovider`共享库（.SO/.DYLIB/.DLL）的非标准位置。

如果未设置此配置变量，则全局环境变量"OPENSSL_MODULES"必须指向`oqsprovider`二进制文件的目录。

如果找不到`oqsprovider`二进制文件，它将简单地（并且静默地）无法使用。

#### 系统范围的安装

系统范围的`openssl.cnf`文件通常位于（取决于操作系统）：
- /etc/ssl/（UNIX/Linux）
- /opt/homebrew/etc/openssl@3/（MacOS Homebrew在Apple Silicon上）
- /usr/local/etc/openssl@3/（MacOS Homebrew在Intel Silicon上）
- C:\Program Files\Common Files\SSL\（Windows）

将`oqsprovider`添加到此文件将使其能够与其他`openssl`提供者无缝运行。如果成功完成，例如，运行 `openssl list -providers` 应该输出以下内容（版本ID变量当然会有所不同）：

```bash
providers:
  default
    name: OpenSSL Default Provider
    version: 3.1.1
    status: active
  oqsprovider
    name: OpenSSL OQS Provider
    version: 0.5.0
    status: active
```

如果是这种情况，所有`openssl`命令都可以像往常一样使用，并通过在使用经典密码算法的同时/替代经典密码算法时，使用量子安全密码算法的选项进行扩展。

这是下面所有示例中使用的配置。

*注意*：请确保始终激活“default”或“fips”提供者，因为这些提供者还提供了`oqsprovider`所需的功能（例如，在密钥生成期间进行哈希处理或生成高质量的随机数据）。

## 选择TLS1.3默认组

要激活特定的[KEMs](README.md#kem-algorithms)，有三个选项：

### 命令行参数

所有允许预先选择KEMs的命令都允许通过标准的OpenSSL [-groups](https://www.openssl.org/docs/manmaster/man3/SSL_CONF_cmd.html)命令行选项进行。例如：

```bash
openssl s_server -cert dilithium3_srv.crt -key dilithium3_srv.key -www -tls1_3 -groups kyber768:frodo640shake
```

### C API

如果要在C程序中硬编码激活特定的KEM组，请使用标准的OpenSSL [SSL_set1_groups_list](https://www.openssl.org/docs/manmaster/man3/SSL_set1_groups_list.html) API。例如：

```c
SSL_set1_groups_list(ssl, "kyber768:kyber1024");
```

### 配置文件参数

可以在`openssl.cnf`文件中设置可接受的KEM组。

例如：

```ini
[openssl_init]
ssl_conf = ssl_sect

[ssl_sect]
system_default = system_default_sect

[system_default_sect]
Groups = kyber768:kyber1024
```

请确保使用冒号分隔多个可接受的KEM名称。

## 示例命令

以下部分提供了用于某些标准OpenSSL操作的示例命令。

### 检查提供者版本信息

```bash
openssl list -providers -verbose
```

### 检查可用于使用的量子安全签名算法

```bash
openssl list -signature-algorithms -provider oqsprovider
```

### 检查可用于使用的量子安全KEM算法

```bash
openssl list -kem-algorithms -provider oqsprovider
```

### 创建密钥和证书

例如，可以使用通常的`openssl`命令简化此过程：

```bash
openssl req -x509 -new -newkey dilithium3 -keyout dilithium3_CA.key -out dilithium3_CA.crt -nodes -subj "/CN=test CA" -days 365 -config openssl/apps/openssl.cnf
openssl genpkey -algorithm dilithium3 -out dilithium3_srv.key
openssl req -new -newkey dilithium3 -keyout dilithium3_srv.key -out dilithium3_srv.csr -nodes -subj "/CN=test server" -config openssl/apps/openssl.cnf
openssl x509 -req -in dilithium3_srv.csr -out dilithium3_srv.crt -CA dilithium3_CA.crt -CAkey dilithium3_CA.key -CAcreateserial -days 365
```

这些示例创建了QSC dilithium3密钥，但可以使用相同的命令使用任何QSC支持的[签名算法](README.md#signature-algorithms)创建QSC证书，当然，也可以使用任何经典签名算法，比如"rsa"。

### 设置（量子安全）测试服务器

使用上述创建的密钥和证书，可以通过运行以下命令来设置一个使用量子安全（QSC）KEM算法和证书的简单服务器：

```bash
openssl s_server -cert dilithium3_srv.crt -key dilithium3_srv.key -www -tls1_3 -groups kyber768:frodo640shake
```

可以使用[QSC签名算法](README.md#signature-algorithms)中的任何一个，以及任何[QSC KEM算法](README.md#kem-algorithms)和经典加密算法。

### 运行与（量子安全）KEM算法交互的客户端

可以通过运行以下命令来运行与（量子安全）KEM算法交互的客户端：

```bash
openssl s_client -groups frodo640shake
```

通过发出 `GET /` 命令，启用量子安全密码学的OpenSSL3服务器会返回有关建立的连接的详细信息。

可以通过在 `-groups` 选项中传递任何[可用的量子安全KEM算法](README.md#kem-algorithms)来选择任何一个。

### S/MIME消息签名 - 密码消息语法（CMS）

#### 签名数据

为了创建签名的数据，需要两个步骤：首先是使用QSC算法创建证书；第二个是使用此证书（及其签名算法）创建签名的数据：

第1步：创建量子安全密钥对和自签名证书：

```bash
openssl req -x509 -new -newkey dilithium3 -keyout qsc.key -out qsc.crt -nodes -subj "/CN=oqstest" -days 365 -config openssl/apps/openssl.cnf
```

通过更改 `-newkey` 参数的算法名，可以使用[支持的任何量子安全或混合算法](README.md#signature-algorithms)来替代示例算法 `dilithium3`。

第2步：签名数据：

由于 [CMS标准](https://datatracker.ietf.org/doc/html/rfc5652#section-5.3) 要求存在摘要算法，而量子安全密码学与上面的QSC证书创建命令不同，必须通过 `-md` 参数传递消息摘要算法是强制性的。

```bash
openssl cms -in inputfile -sign -signer qsc.crt -inkey qsc.key -nodetach -outform pem -binary -out signedfile -md sha512
```

要签名的数据应该包含在名为 `inputfile` 的文件中。生成的CMS输出包含在文件 `signedfile` 中。使用的QSC算法与用于签署证书 `qsc.crt` 的签名算法相同。

#### 验证数据

在上述示例的基础上，以下命令验证CMS文件 `signedfile` 并输出 `outputfile`。其内容应该与上述 `inputfile` 中的原始数据相同。

```bash
openssl cms -verify -CAfile qsc.crt -inform pem -in signedfile -crlfeol -out outputfile
```

还可以使用标准的OpenSSL调用构建正确的QSC证书链。有关示例代码，请参见 [scripts/oqsprovider-certgen.sh](scripts/oqsprovider-certgen.sh)。

### `dgst`（和签名）的支持

还测试了[openssl dgst](https://www.openssl.org/docs/man3.0/man1/openssl-dgst.html)命令的操作。以下是建立在上述示例中的密钥和证书文件基础上的示例调用：

#### 签名

```bash
openssl dgst -sign qsc.key -out dgstsignfile inputfile
```

#### 验证

```bash
openssl dgst -signature dgstsignfile -verify qsc.pubkey inputfile
```

可以使用标准的openssl命令从证书中提取公钥：

```bash
openssl x509 -in qsc.crt -pubkey -noout > qsc.pubkey
```

`dgst`命令未经[oqs-openssl111](https://github.com/open-quantum-safe/openssl)测试与其互操作性。

*关于KEM解封装API的说明*：

OpenSSL的[`EVP_PKEY_decapsulate` API](https://www.openssl.org/docs/manmaster/man3/EVP_PKEY_decapsulate.html)为失败指定了明确的返回值。出于安全原因，来自liboqs的大多数KEM算法在解封装失败时不会返回错误代码。相反，可以通过比较原始消息和解封装后的消息来隐式验证成功的解封装。