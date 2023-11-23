[![GitHub actions](https://github.com/open-quantum-safe/oqs-provider/actions/workflows/linux.yml/badge.svg)](https://github.com/open-quantum-safe/oqs-provider/actions/workflows/linux.yml)
[![GitHub actions](https://github.com/open-quantum-safe/oqs-provider/actions/workflows/windows.yml/badge.svg)](https://github.com/open-quantum-safe/oqs-provider/actions/workflows/windows.yml)
[![GitHub actions](https://github.com/open-quantum-safe/oqs-provider/actions/workflows/macos.yml/badge.svg)](https://github.com/open-quantum-safe/oqs-provider/actions/workflows/macos.yml)
[![oqs-provider](https://circleci.com/gh/open-quantum-safe/oqs-provider.svg?style=svg)](https://app.circleci.com/pipelines/github/open-quantum-safe/oqs-provider)

oqsprovider - OpenSSL (3.x)的Open Quantum Safe提供程序
=====================================================

目的
----

该存储库包含使量子安全密码学（QSC）能够在标准的OpenSSL（3.x）分发中实现的代码，通过实现一个单独的共享库，即OQS [provider](https://www.openssl.org/docs/manmaster/man7/provider.html)。

状态
----

当前，该提供程序完全支持TLS1.3中的KEM密钥建立的量子安全密码学，包括通过OpenSSL（3.0）提供程序接口管理这些密钥以及混合KEM方案。此外，通过OpenSSL EVP接口提供了QSC签名，包括CMS和CMP功能。密钥持久性通过编码/解码机制和X.509数据结构提供。从OpenSSL 3.2开始，支持TLS1.3签名功能，并解决了CMS的最后一些问题。

实现的标准在单独的文件[STANDARDS.md](STANDARDS.md)中有文档。

算法
----

此实现提供以下量子安全算法：

<!--- OQS_TEMPLATE_FRAGMENT_ALGS_START -->
### KEM算法

- **BIKE**: `bikel1`, `p256_bikel1`, `x25519_bikel1`, `bikel3`, `p384_bikel3`, `x448_bikel3`, `bikel5`, `p521_bikel5`
- **CRYSTALS-Kyber**: `kyber512`, `p256_kyber512`, `x25519_kyber512`, `kyber768`, `p384_kyber768`, `x448_kyber768`, `x25519_kyber768`, `p256_kyber768`, `kyber1024`, `p521_kyber1024`
- **FrodoKEM**: `frodo640aes`, `p256_frodo640aes`, `x25519_frodo640aes`, `frodo640shake`, `p256_frodo640shake`, `x25519_frodo640shake`, `frodo976aes`, `p384_frodo976aes`, `x448_frodo976aes`, `frodo976shake`, `p384_frodo976shake`, `x448_frodo976shake`, `frodo1344aes`, `p521_frodo1344aes`, `frodo1344shake`, `p521_frodo1344shake`
- **HQC**: `hqc128`, `p256_hqc128`, `x25519_hqc128`, `hqc192`, `p384_hqc192`, `x448_hqc192`, `hqc256`, `p521_hqc256`†

### 签名算法

- **CRYSTALS-Dilithium**:`dilithium2`\*, `p256_dilithium2`\*, `rsa3072_dilithium2`\*, `dilithium3`\*, `p384_dilithium3`\*, `dilithium5`\*, `p521_dilithium5`\*
- **Falcon**:`falcon512`\*, `p256_falcon512`\*, `rsa3072_falcon512`\*, `falcon1024`\*, `p521_falcon1024`\*

- **SPHINCS-SHA2**:`sphincssha2128fsimple`\*, `p256_sphincssha2128fsimple`\*, `rsa3072_sphincssha2128fsimple`\*, `sphincssha2128ssimple`\*, `p256_sphincssha2128ssimple`\*, `rsa3072_sphincssha2128ssimple`\*, `sphincssha2192fsimple`\*, `p384_sphincssha2192fsimple`\*, `sphincssha2192ssimple`, `p384_sphincssha2192ssimple`, `sphincssha2256fsimple`, `p521_sphincssha2256fsimple`, `sphincssha2256ssimple`, `p521_sphincssha2256ssimple`
- **SPHINCS-SHAKE**:`sphincsshake128fsimple`\*, `p256_sphincsshake128fsimple`\*, `rsa3072_sphincsshake128fsimple`\*, `sphincsshake128ssimple`, `p256_sphincsshake128ssimple`, `rsa3072_sphincsshake128ssimple`, `sphincsshake192fsimple`, `p384_sphincsshake192fsimple`, `sphincsshake192ssimple`, `p384_sphincsshake192ssimple`, `sphincsshake256fsimple`, `p521_sphincsshake256fsimple`, `sphincsshake256ssimple`, `p521_sphincsshake256ssimple`

<!--- OQS_TEMPLATE_FRAGMENT_ALGS_END -->

由于在构建时[liboqs](https://github.com/open-quantum-safe/liboqs)可以配置为不启用所有算法，建议使用标准命令检查实际启用的算法的可能子集，即`openssl list -signature-algorithms -provider oqsprovider`和`openssl list -kem-algorithms -provider oqsprovider`。

此外，未在上面用"\*"标注的算法不支持TLS操作。此标记[可以通过修改主算法配置文件中的"enabled"标志来更改](CONFIGURE.md#pre-build-configuration)。

为了支持经典和量子安全密码学的并行使用，该提供程序还提供了不同的混合算法，将经典和量子安全方法结合起来：这些算法在前面带有经典算法的前缀，例如，对于椭圆曲线："p256_"。

算法的完整列表、它们的互操作性代码点和OID，以及动态调整它们的方法，例如，用于互操作性测试的方法都在[ALGORITHMS.md](ALGORITHMS.md)中有文档。

构建和测试 -- 快速入门
-----------------------

所有在下面详细描述的组件构建和测试都可以通过运行脚本`scripts/fullbuild.sh`和`scripts/runtests.sh`来执行（在Linux Ubuntu和Mint以及MacOS上进行了测试）。

默认情况下，这些脚本始终针对当前的OpenSSL `master`分支进行构建和测试。

这些脚本可以通过[设置各种变量](CONFIGURE.md#convenience-build-script-options)进行配置。请注意，这些脚本不会安装`oqsprovider`。这可以通过运行`cmake --install _build`来实现（并遵循[激活说明](USAGE.md#activation)）。

构建和测试
-----------

以下描述了使用标准`cmake`工具进行基本构建-测试-安装循环的步骤。有关UNIX（包括MacOS和`cygwin`）和Windows的平台特定说明，请参阅[NOTES-UNIX.md](NOTES-UNIX.md)和[NOTES-Windows.md](NOTES-Windows.md)。

## 配置选项

有关在构建或运行时配置`oqs-provider`的所有选项的文档，请参阅[CONFIGURE.md](CONFIGURE.md)。

## 先决条件

要能够构建`oqsprovider`，需要安装OpenSSL 3.0和liboqs。重要的是它们安装在哪里，只要它们被安装即可。如果安装在非标准位置，必须在运行`cmake`时提供这些位置，通过变量"OPENSSL_ROOT_DIR"和"liboqs_DIR"。详细信息请参阅[CONFIGURE.md](CONFIGURE.md)。

## 基本步骤

    cmake -S . -B _build && cmake --build _build && ctest --test-dir _build && cmake --install _build
    
使用
-----

`oqsprovider`的使用在单独的[USAGE.md](USAGE.md)文件中有文档。

关于OpenSSL版本的说明
------------------------

`oqsprovider`被设计为确保在支持提供程序概念的所有OpenSSL版本上构建。然而，关于通过提供程序接口支持的功能，OpenSSL仍在积极开发中。因此，上述文档中仅特定于某些OpenSSL版本的支持某些功能的说明：

## 3.0/3.1

在这些版本中，提供程序中实现的CMS功能不受支持：https://github.com/openssl/openssl/issues/17717 的解决方案未被回溯到OpenSSL3.0。

此版本中还不支持在TLS1.3操作中使用提供程序的签名算法，如https://github.com/openssl/openssl/issues/10512中所述。

## 3.2(-dev)

在https://github.com/openssl/openssl/pull/19312成功后，（也是PQ）签名算法在TLS1.3（握手）中工作；在https://github.com/openssl/openssl/pull/20486成功后，还支持具有非常长签名的算法。

关于[一般的OpenSSL实现限制，例如，关于提供程序功能使用和支持的信息，请参见此处](https://wiki.openssl.org/index.php/OpenSSL_3.0#STATUS_of_current_development)。

团队
----

Open Quantum Safe项目由滑铁卢大学的[Douglas Stebila](https://www.douglas.stebila.ca/research/)和[Michele Mosca](http://faculty.iqc.uwaterloo.ca/mmosca/)领导。

`oqsprovider`的贡献者包括：

- Michael Baentsch
- Christian Paquin
- Richard Levitte
- Basil Hess
- Julian Segeth
- Alex Zaslavsky
- Will Childs-Klein
- Thomas Bailleux

历史
----

当前和过去版本（“代码历史”）的文档在单独的[RELEASE.md](RELEASE.md)文件中有文档。

致谢
----

`oqsprovider`项目通过[NGI Assure基金](https://nlnet.nl/assure)得到支持，这是由[NLnet](https://nlnet.nl)建立的一个基金，由欧洲委员会的[下一代互联网计划](https://www.ngi.eu)提供财政支持，隶属于DG Communications Networks、内容和技术在合同编号957073下。

Open Quantum Safe项目的发展得到了Amazon Web Services和Tutte Institute for Mathematics and Computing的财政支持。

为Open Quantum Safe的开发提供了特定组件的程序员时间的公司，包括Amazon Web Services、evolutionQ、Microsoft Research、Cisco Systems和IBM Research在内，都得到了特别的致谢。

开发OQS的特定组件的研究项目得到了各种研究经费的支持，包括加拿大自然科学和工程研究委员会（NSERC）的资助；请参见[此处](https://openquantumsafe.org/papers/SAC-SteMos16.pdf)和[此处](https://openquantumsafe.org/papers/NISTPQC-CroPaqSte19.pdf)以获取资助方面的感谢。

# 声明

## 标准软件声明

本软件不附带任何明示或暗示的保证，所有暗示的保证均被拒绝，包括任何对适销性和特定用途的保证。

## 组件声明
[liboqs disclaimer](https://github.com/open-quantum-safe/liboqs#limitations-and-security)