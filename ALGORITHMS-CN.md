算法支持
====================


这篇文档描述了 oqs-provider 支持的所有量子安全算法。在 TLS 标准化完成之前，某些算法可能默认情况下未启用，但可以通过设置环境变量更改其在主代码生成器模板文件中的使用情况。所有 TLS 代码点/ID 都可以从其默认值更改为环境变量设置的值，以促进与使用不同 ID 的 TLS 1.3 实现的互操作性测试。

## 代码点/算法 ID

以下是支持的算法列表，包括默认 ID、启用状态以及相关的环境变量。

| 算法名称                       | 默认 ID | 是否启用 | 环境变量                                     |
| ------------------------------ | :-----: | :------: | -------------------------------------------- |
| frodo640aes                    | 0x0200  |   Yes    | OQS_CODEPOINT_FRODO640AES                    |
| p256_frodo640aes               | 0x2F00  |   Yes    | OQS_CODEPOINT_P256_FRODO640AES               |
| x25519_frodo640aes             | 0x2F80  |   Yes    | OQS_CODEPOINT_X25519_FRODO640AES             |
| frodo640shake                  | 0x0201  |   Yes    | OQS_CODEPOINT_FRODO640SHAKE                  |
| p256_frodo640shake             | 0x2F01  |   Yes    | OQS_CODEPOINT_P256_FRODO640SHAKE             |
| x25519_frodo640shake           | 0x2F81  |   Yes    | OQS_CODEPOINT_X25519_FRODO640SHAKE           |
| frodo976aes                    | 0x0202  |   Yes    | OQS_CODEPOINT_FRODO976AES                    |
| p384_frodo976aes               | 0x2F02  |   Yes    | OQS_CODEPOINT_P384_FRODO976AES               |
| x448_frodo976aes               | 0x2F82  |   Yes    | OQS_CODEPOINT_X448_FRODO976AES               |
| frodo976shake                  | 0x0203  |   Yes    | OQS_CODEPOINT_FRODO976SHAKE                  |
| p384_frodo976shake             | 0x2F03  |   Yes    | OQS_CODEPOINT_P384_FRODO976SHAKE             |
| x448_frodo976shake             | 0x2F83  |   Yes    | OQS_CODEPOINT_X448_FRODO976SHAKE             |
| frodo1344aes                   | 0x0204  |   Yes    | OQS_CODEPOINT_FRODO1344AES                   |
| p521_frodo1344aes              | 0x2F04  |   Yes    | OQS_CODEPOINT_P521_FRODO1344AES              |
| frodo1344shake                 | 0x0205  |   Yes    | OQS_CODEPOINT_FRODO1344SHAKE                 |
| p521_frodo1344shake            | 0x2F05  |   Yes    | OQS_CODEPOINT_P521_FRODO1344SHAKE            |
| kyber512                       | 0x023A  |   Yes    | OQS_CODEPOINT_KYBER512                       |
| p256_kyber512                  | 0x2F3A  |   Yes    | OQS_CODEPOINT_P256_KYBER512                  |
| x25519_kyber512                | 0x2F39  |   Yes    | OQS_CODEPOINT_X25519_KYBER512                |
| kyber768                       | 0x023C  |   Yes    | OQS_CODEPOINT_KYBER768                       |
| p384_kyber768                  | 0x2F3C  |   Yes    | OQS_CODEPOINT_P384_KYBER768                  |
| x448_kyber768                  | 0x2F90  |   Yes    | OQS_CODEPOINT_X448_KYBER768                  |
| x25519_kyber768                | 0x6399  |   Yes    | OQS_CODEPOINT_X25519_KYBER768                |
| p256_kyber768                  | 0x639A  |   Yes    | OQS_CODEPOINT_P256_KYBER768                  |
| kyber1024                      | 0x023D  |   Yes    | OQS_CODEPOINT_KYBER1024                      |
| p521_kyber1024                 | 0x2F3D  |   Yes    | OQS_CODEPOINT_P521_KYBER1024                 |
| bikel1                         | 0x0241  |   Yes    | OQS_CODEPOINT_BIKEL1                         |
| p256_bikel1                    | 0x2F41  |   Yes    | OQS_CODEPOINT_P256_BIKEL1                    |
| x25519_bikel1                  | 0x2FAE  |   Yes    | OQS_CODEPOINT_X25519_BIKEL1                  |
| bikel3                         | 0x0242  |   Yes    | OQS_CODEPOINT_BIKEL3                         |
| p384_bikel3                    | 0x2F42  |   Yes    | OQS_CODEPOINT_P384_BIKEL3                    |
| x448_bikel3                    | 0x2FAF  |   Yes    | OQS_CODEPOINT_X448_BIKEL3                    |
| bikel5                         | 0x0243  |   Yes    | OQS_CODEPOINT_BIKEL5                         |
| p521_bikel5                    | 0x2F43  |   Yes    | OQS_CODEPOINT_P521_BIKEL5                    |
| hqc128                         | 0x022C  |   Yes    | OQS_CODEPOINT_HQC128                         |
| p256_hqc128                    | 0x2F2C  |   Yes    | OQS_CODEPOINT_P256_HQC128                    |
| x25519_hqc128                  | 0x2FAC  |   Yes    | OQS_CODEPOINT_X25519_HQC128                  |
| hqc192                         | 0x022D  |   Yes    | OQS_CODEPOINT_HQC192                         |
| p384_hqc192                    | 0x2F2D  |   Yes    | OQS_CODEPOINT_P384_HQC192                    |
| x448_hqc192                    | 0x2FAD  |   Yes    | OQS_CODEPOINT_X448_HQC192                    |
| hqc256                         | 0x022E  |   Yes    | OQS_CODEPOINT_HQC256                         |
| p521_hqc256                    | 0x2F2E  |   Yes    | OQS_CODEPOINT_P521_HQC256                    |
| dilithium2                     | 0xfea0  |   Yes    | OQS_CODEPOINT_DILITHIUM2                     |
| p256_dilithium2                | 0xfea1  |   Yes    | OQS_CODEPOINT_P256_DILITHIUM2                |
| rsa3072_dilithium2             | 0xfea2  |   Yes    | OQS_CODEPOINT_RSA3072_DILITHIUM2             |
| dilithium3                     | 0xfea3  |   Yes    | OQS_CODEPOINT_DILITHIUM3                     |
| p384_dilithium3                | 0xfea4  |   Yes    | OQS_CODEPOINT_P384_DILITHIUM3                |
| dilithium5                     | 0xfea5  |   Yes    | OQS_CODEPOINT_DILITHIUM5                     |
| p521_dilithium5                | 0xfea6  |   Yes    | OQS_CODEPOINT_P521_DILITHIUM5                |
| falcon512                      | 0xfeae  |   Yes    | OQS_CODEPOINT_FALCON512                      |
| p256_falcon512                 | 0xfeaf  |   Yes    | OQS_CODEPOINT_P256_FALCON512                 |
| rsa3072_falcon512              | 0xfeb0  |   Yes    | OQS_CODEPOINT_RSA3072_FALCON512              |
| falcon1024                     | 0xfeb1  |   Yes    | OQS_CODEPOINT_FALCON1024                     |
| p521_falcon1024                | 0xfeb2  |   Yes    | OQS_CODEPOINT_P521_FALCON1024                |
| sphincssha2128fsimple          | 0xfeb3  |   Yes    | OQS_CODEPOINT_SPHINCSSHA2128FSIMPLE          |
| p256_sphincssha2128fsimple     | 0xfeb4  |   Yes    | OQS_CODEPOINT_P256_SPHINCSSHA2128FSIMPLE     |
| rsa3072_sphincssha2128fsimple  | 0xfeb5  |   Yes    | OQS_CODEPOINT_RSA3072_SPHINCSSHA2128FSIMPLE  |
| sphincssha2128ssimple          | 0xfeb6  |   Yes    | OQS_CODEPOINT_SPHINCSSHA2128SSIMPLE          |
| p256_sphincssha2128ssimple     | 0xfeb7  |   Yes    | OQS_CODEPOINT_P256_SPHINCSSHA2128SSIMPLE     |
| rsa3072_sphincssha2128ssimple  | 0xfeb8  |   Yes    | OQS_CODEPOINT_RSA3072_SPHINCSSHA2128SSIMPLE  |
| sphincssha2192fsimple          | 0xfeb9  |   Yes    | OQS_CODEPOINT_SPHINCSSHA2192FSIMPLE          |
| p384_sphincssha2192fsimple     | 0xfeba  |   Yes    | OQS_CODEPOINT_P384_SPHINCSSHA2192FSIMPLE     |
| sphincssha2192ssimple          | 0xfebb  |    No    | OQS_CODEPOINT_SPHINCSSHA2192SSIMPLE          |
| p384_sphincssha2192ssimple     | 0xfebc  |    No    | OQS_CODEPOINT_P384_SPHINCSSHA2192SSIMPLE     |
| sphincssha2256fsimple          | 0xfebd  |    No    | OQS_CODEPOINT_SPHINCSSHA2256FSIMPLE          |
| p521_sphincssha2256fsimple     | 0xfebe  |    No    | OQS_CODEPOINT_P521_SPHINCSSHA2256FSIMPLE     |
| sphincssha2256ssimple          | 0xfec0  |    No    | OQS_CODEPOINT_SPHINCSSHA2256SSIMPLE          |
| p521_sphincssha2256ssimple     | 0xfec1  |    No    | OQS_CODEPOINT_P521_SPHINCSSHA2256SSIMPLE     |
| sphincsshake128fsimple         | 0xfec2  |   Yes    | OQS_CODEPOINT_SPHINCSSHAKE128FSIMPLE         |
| p256_sphincsshake128fsimple    | 0xfec3  |   Yes    | OQS_CODEPOINT_P256_SPHINCSSHAKE128FSIMPLE    |
| rsa3072_sphincsshake128fsimple | 0xfec4  |   Yes    | OQS_CODEPOINT_RSA3072_SPHINCSSHAKE128FSIMPLE |
| sphincsshake128ssimple         | 0xfec5  |    No    | OQS_CODEPOINT_SPHINCSSHAKE128SSIMPLE         |
| p256_sphincsshake128ssimple    | 0xfec6  |    No    | OQS_CODEPOINT_P256_SPHINCSSHAKE128SSIMPLE    |
| rsa3072_sphincsshake128ssimple | 0xfec7  |    No    | OQS_CODEPOINT_RSA3072_SPHINCSSHAKE128SSIMPLE |
| sphincsshake192fsimple         | 0xfec8  |    No    | OQS_CODEPOINT_SPHINCSSHAKE192FSIMPLE         |
| p384_sphincsshake192fsimple    | 0xfec9  |    No    | OQS_CODEPOINT_P384_SPHINCSSHAKE192FSIMPLE    |
| sphincsshake192ssimple         | 0xfeca  |    No    | OQS_CODEPOINT_SPHINCSSHAKE192SSIMPLE         |
| p384_sphincsshake192ssimple    | 0xfecb  |    No    | OQS_CODEPOINT_P384_SPHINCSSHAKE192SSIMPLE    |
| sphincsshake256fsimple         | 0xfecc  |    No    | OQS_CODEPOINT_SPHINCSSHAKE256FSIMPLE         |
| p521_sphincsshake256fsimple    | 0xfecd  |    No    | OQS_CODEPOINT_P521_SPHINCSSHAKE256FSIMPLE    |
| sphincsshake256ssimple         | 0xfece  |    No    | OQS_CODEPOINT_SPHINCSSHAKE256SSIMPLE         |
| p521_sphincsshake256ssimple    | 0xfecf  |    No    | OQS_CODEPOINT_P521_SPHINCSSHAKE256SSIMPLE    |

Changing code points
--------------------
要动态更改任何算法的代码点，必须将上述相应的环境变量设置为所需代码点的整数值。例如，如果 Cloudflare 已选择 `0xfe30` 作为其 X25519_kyber512 实现的代码点，可以使用以下命令成功确认与 Cloudflare 基础架构之间的 oqs-provider 和 TLS 互操作性：

```bash
OQS_CODEPOINT_X25519_KYBER512=65072  ./openssl/apps/openssl s_client -groups x25519_kyber512 -connect cloudflare.com:443 -provider-path _build/oqsprov -provider oqsprovider -provider default
```

## OID

此外，与代码点类似，X.509 对象标识符（OID）可能在最终标准化之前发生变化。以下环境变量允许根据下表调整所有支持的签名算法的 OID。

| 算法名称                       |        默认 OID         | 是否启用 | 环境变量                               |
| ------------------------------ | :---------------------: | :------: | -------------------------------------- |
| dilithium2                     | 1.3.6.1.4.1.2.267.7.4.4 |   Yes    | OQS_OID_DILITHIUM2                     |
| p256_dilithium2                |     1.3.9999.2.7.1      |   Yes    | OQS_OID_P256_DILITHIUM2                |
| rsa3072_dilithium2             |     1.3.9999.2.7.2      |   Yes    | OQS_OID_RSA3072_DILITHIUM2             |
| dilithium3                     | 1.3.6.1.4.1.2.267.7.6.5 |   Yes    | OQS_OID_DILITHIUM3                     |
| p384_dilithium3                |     1.3.9999.2.7.3      |   Yes    | OQS_OID_P384_DILITHIUM3                |
| dilithium5                     | 1.3.6.1.4.1.2.267.7.8.7 |   Yes    | OQS_OID_DILITHIUM5                     |
| p521_dilithium5                |     1.3.9999.2.7.4      |   Yes    | OQS_OID_P521_DILITHIUM5                |
| falcon512                      |      1.3.9999.3.6       |   Yes    | OQS_OID_FALCON512                      |
| p256_falcon512                 |      1.3.9999.3.7       |   Yes    | OQS_OID_P256_FALCON512                 |
| rsa3072_falcon512              |      1.3.9999.3.8       |   Yes    | OQS_OID_RSA3072_FALCON512              |
| falcon1024                     |      1.3.9999.3.9       |   Yes    | OQS_OID_FALCON1024                     |
| p521_falcon1024                |      1.3.9999.3.10      |   Yes    | OQS_OID_P521_FALCON1024                |
| sphincssha2128fsimple          |     1.3.9999.6.4.13     |   Yes    | OQS_OID_SPHINCSSHA2128FSIMPLE          |
| p256_sphincssha2128fsimple     |     1.3.9999.6.4.14     |   Yes    | OQS_OID_P256_SPHINCSSHA2128FSIMPLE     |
| rsa3072_sphincssha2128fsimple  |     1.3.9999.6.4.15     |   Yes    | OQS_OID_RSA3072_SPHINCSSHA2128FSIMPLE  |
| sphincssha2128ssimple          |     1.3.9999.6.4.16     |   Yes    | OQS_OID_SPHINCSSHA2128SSIMPLE          |
| p256_sphincssha2128ssimple     |     1.3.9999.6.4.17     |   Yes    | OQS_OID_P256_SPHINCSSHA2128SSIMPLE     |
| rsa3072_sphincssha2128ssimple  |     1.3.9999.6.4.18     |   Yes    | OQS_OID_RSA3072_SPHINCSSHA2128SSIMPLE  |
| sphincssha2192fsimple          |     1.3.9999.6.5.10     |   Yes    | OQS_OID_SPHINCSSHA2192FSIMPLE          |
| p384_sphincssha2192fsimple     |     1.3.9999.6.5.11     |   Yes    | OQS_OID_P384_SPHINCSSHA2192FSIMPLE     |
| sphincssha2192ssimple          |     1.3.9999.6.5.12     |    No    | OQS_OID_SPHINCSSHA2192SSIMPLE          |
| p384_sphincssha2192ssimple     |     1.3.9999.6.5.13     |    No    | OQS_OID_P384_SPHINCSSHA2192SSIMPLE     |
| sphincssha2256fsimple          |     1.3.9999.6.6.10     |    No    | OQS_OID_SPHINCSSHA2256FSIMPLE          |
| p521_sphincssha2256fsimple     |     1.3.9999.6.6.11     |    No    | OQS_OID_P521_SPHINCSSHA2256FSIMPLE     |
| sphincssha2256ssimple          |     1.3.9999.6.6.12     |    No    | OQS_OID_SPHINCSSHA2256SSIMPLE          |
| p521_sphincssha2256ssimple     |     1.3.9999.6.6.13     |    No    | OQS_OID_P521_SPHINCSSHA2256SSIMPLE     |
| sphincsshake128fsimple         |     1.3.9999.6.7.13     |   Yes    | OQS_OID_SPHINCSSHAKE128FSIMPLE         |
| p256_sphincsshake128fsimple    |     1.3.9999.6.7.14     |   Yes    | OQS_OID_P256_SPHINCSSHAKE128FSIMPLE    |
| rsa3072_sphincsshake128fsimple |     1.3.9999.6.7.15     |   Yes    | OQS_OID_RSA3072_SPHINCSSHAKE128FSIMPLE |
| sphincsshake128ssimple         |     1.3.9999.6.7.16     |    No    | OQS_OID_SPHINCSSHAKE128SSIMPLE         |
| p256_sphincsshake128ssimple    |     1.3.9999.6.7.17     |    No    | OQS_OID_P256_SPHINCSSHAKE128SSIMPLE    |
| rsa3072_sphincsshake128ssimple |     1.3.9999.6.7.18     |    No    | OQS_OID_RSA3072_SPHINCSSHAKE128SSIMPLE |
| sphincsshake192fsimple         |     1.3.9999.6.8.10     |    No    | OQS_OID_SPHINCSSHAKE192FSIMPLE         |
| p384_sphincsshake192fsimple    |     1.3.9999.6.8.11     |    No    | OQS_OID_P384_SPHINCSSHAKE192FSIMPLE    |
| sphincsshake192ssimple         |     1.3.9999.6.8.12     |    No    | OQS_OID_SPHINCSSHAKE192SSIMPLE         |
| p384_sphincsshake192ssimple    |     1.3.9999.6.8.13     |    No    | OQS_OID_P384_SPHINCSSHAKE192SSIMPLE    |
| sphincsshake256fsimple         |     1.3.9999.6.9.10     |    No    | OQS_OID_SPHINCSSHAKE256FSIMPLE         |
| p521_sphincsshake256fsimple    |     1.3.9999.6.9.11     |    No    | OQS_OID_P521_SPHINCSSHAKE256FSIMPLE    |
| sphincsshake256ssimple         |     1.3.9999.6.9.12     |    No    | OQS_OID_SPHINCSSHAKE256SSIMPLE         |
| p521_sphincsshake256ssimple    |     1.3.9999.6.9.13     |    No    | OQS_OID_P521_SPHINCSSHAKE256SSIMPLE    |

If [OQS_KEM_ENCODERS](CONFIGURE.md#OQS_KEM_ENCODERS) is enabled the following list is also available:

| Algorithm name       |       default OID       | environment variable         |
| -------------------- | :---------------------: | ---------------------------- |
| frodo640aes          |     1.3.9999.99.50      | OQS_OID_FRODO640AES          |
| p256_frodo640aes     |     1.3.9999.99.49      | OQS_OID_P256_FRODO640AES     |
| x25519_frodo640aes   |     1.3.9999.99.38      | OQS_OID_X25519_FRODO640AES   |
| frodo640shake        |     1.3.9999.99.52      | OQS_OID_FRODO640SHAKE        |
| p256_frodo640shake   |     1.3.9999.99.51      | OQS_OID_P256_FRODO640SHAKE   |
| x25519_frodo640shake |     1.3.9999.99.39      | OQS_OID_X25519_FRODO640SHAKE |
| frodo976aes          |     1.3.9999.99.54      | OQS_OID_FRODO976AES          |
| p384_frodo976aes     |     1.3.9999.99.53      | OQS_OID_P384_FRODO976AES     |
| x448_frodo976aes     |     1.3.9999.99.40      | OQS_OID_X448_FRODO976AES     |
| frodo976shake        |     1.3.9999.99.56      | OQS_OID_FRODO976SHAKE        |
| p384_frodo976shake   |     1.3.9999.99.55      | OQS_OID_P384_FRODO976SHAKE   |
| x448_frodo976shake   |     1.3.9999.99.41      | OQS_OID_X448_FRODO976SHAKE   |
| frodo1344aes         |     1.3.9999.99.58      | OQS_OID_FRODO1344AES         |
| p521_frodo1344aes    |     1.3.9999.99.57      | OQS_OID_P521_FRODO1344AES    |
| frodo1344shake       |     1.3.9999.99.60      | OQS_OID_FRODO1344SHAKE       |
| p521_frodo1344shake  |     1.3.9999.99.59      | OQS_OID_P521_FRODO1344SHAKE  |
| kyber512             | 1.3.6.1.4.1.22554.5.6.1 | OQS_OID_KYBER512             |
| p256_kyber512        | 1.3.6.1.4.1.22554.5.7.1 | OQS_OID_P256_KYBER512        |
| x25519_kyber512      | 1.3.6.1.4.1.22554.5.8.1 | OQS_OID_X25519_KYBER512      |
| kyber768             | 1.3.6.1.4.1.22554.5.6.2 | OQS_OID_KYBER768             |
| p384_kyber768        |     1.3.9999.99.61      | OQS_OID_P384_KYBER768        |
| x448_kyber768        |     1.3.9999.99.42      | OQS_OID_X448_KYBER768        |
| x25519_kyber768      |     1.3.9999.99.43      | OQS_OID_X25519_KYBER768      |
| p256_kyber768        |     1.3.9999.99.44      | OQS_OID_P256_KYBER768        |
| kyber1024            | 1.3.6.1.4.1.22554.5.6.3 | OQS_OID_KYBER1024            |
| p521_kyber1024       |     1.3.9999.99.62      | OQS_OID_P521_KYBER1024       |
| bikel1               |     1.3.9999.99.64      | OQS_OID_BIKEL1               |
| p256_bikel1          |     1.3.9999.99.63      | OQS_OID_P256_BIKEL1          |
| x25519_bikel1        |     1.3.9999.99.45      | OQS_OID_X25519_BIKEL1        |
| bikel3               |     1.3.9999.99.66      | OQS_OID_BIKEL3               |
| p384_bikel3          |     1.3.9999.99.65      | OQS_OID_P384_BIKEL3          |
| x448_bikel3          |     1.3.9999.99.46      | OQS_OID_X448_BIKEL3          |
| bikel5               |     1.3.9999.99.68      | OQS_OID_BIKEL5               |
| p521_bikel5          |     1.3.9999.99.67      | OQS_OID_P521_BIKEL5          |
| hqc128               |     1.3.9999.99.70      | OQS_OID_HQC128               |
| p256_hqc128          |     1.3.9999.99.69      | OQS_OID_P256_HQC128          |
| x25519_hqc128        |     1.3.9999.99.47      | OQS_OID_X25519_HQC128        |
| hqc192               |     1.3.9999.99.72      | OQS_OID_HQC192               |
| p384_hqc192          |     1.3.9999.99.71      | OQS_OID_P384_HQC192          |
| x448_hqc192          |     1.3.9999.99.48      | OQS_OID_X448_HQC192          |
| hqc256               |     1.3.9999.99.74      | OQS_OID_HQC256               |
| p521_hqc256          |     1.3.9999.99.73      | OQS_OID_P521_HQC256          |
<!--- OQS_TEMPLATE_FRAGMENT_OIDS_END -->

## 密钥编码

oqs-provider 可以根据以下 IETF 草案配置环境变量以根据特定 IETF 草案对密钥（subjectPublicKey 和 privateKey ASN.1 结构）进行编码：

- [draft-uni-qsckeys-dilithium](https://datatracker.ietf.org/doc/draft-uni-qsckeys-dilithium/00/)
- [draft-uni-qsckeys-falcon](https://datatracker.ietf.org/doc/draft-uni-qsckeys-falcon/00/)
- [draft-uni-qsckeys-sphincsplus](https://datatracker.ietf.org/doc/draft-uni-qsckeys-sphincsplus/00/)

以下是支持的编码及其相应的环境变量：

| 环境变量                              | 允许的值                                 |
| ------------------------------------- | ---------------------------------------- |
| `OQS_ENCODING_DILITHIUM2`             | `draft-uni-qsckeys-dilithium-00/sk-pk`   |
| `OQS_ENCODING_DILITHIUM3`             | `draft-uni-qsckeys-dilithium-00/sk-pk`   |
| `OQS_ENCODING_DILITHIUM5`             | `draft-uni-qsckeys-dilithium-00/sk-pk`   |
| `OQS_ENCODING_FALCON512`              | `draft-uni-qsckeys-falcon-00/sk-pk`      |
| `OQS_ENCODING_FALCON1024`             | `draft-uni-qsckeys-falcon-00/sk-pk`      |
| `OQS_ENCODING_SPHINCSSHA2128FSIMPLE`  | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHA2128SSIMPLE`  | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHA2192FSIMPLE`  | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHA2192SSIMPLE`  | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHA2256FSIMPLE`  | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHA2256SSIMPLE`  | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHAKE128FSIMPLE` | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHAKE128SSIMPLE` | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHAKE192FSIMPLE` | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHAKE192SSIMPLE` | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHAKE256FSIMPLE` | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
| `OQS_ENCODING_SPHINCSSHAKE256SSIMPLE` | `draft-uni-qsckeys-sphincsplus-00/sk-pk` |
<!--- OQS_TEMPLATE_FRAGMENT_ENCODINGS_END -->

通过设置`OQS_ENCODING_<ALGORITHM>_ALGNAME`环境变量，可以设置相应的算法名称。这些名称在编码器库的[`qsc_encoding.h`](https://github.com/Quantum-Safe-Collaboration/qsc-key-encoder/blob/main/include/qsc_encoding.h)头文件中有文档记录。

如果没有设置环境变量，或者设置了未知的值，那么默认情况下是没有编码的，这意味着密钥序列化使用密码实现的“原始”密钥。如果将未知值设置为环境变量，将引发运行时错误。

测试脚本 `scripts/runtests_encodings.sh`（而不是 `scripts/runtests.sh`）可用于激活所有支持的编码进行测试运行。