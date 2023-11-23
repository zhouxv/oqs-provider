# 支持的标准：部署 oqsprovider

对于非后量子算法，此提供程序基本上是静默的，即允许使用由 [openssl](https://github.com/openssl/openssl) 实现的标准和算法，例如涉及 X.509、PKCS#8 或 CMS 的标准。

对于后量子算法，所使用的加密算法版本取决于使用的 [liboqs](https://github.com/open-quantum-safe/liboqs) 版本。关于将后量子算法集成到更高级别组件中，此提供程序实现了以下标准：

- 对于 TLS：
  - 混合后量子/传统密钥交换：
    - 所使用的数据结构遵循互联网草案 [TLS 1.3 中的混合密钥交换](https://datatracker.ietf.org/doc/draft-ietf-tls-hybrid-design/)，即传统和后量子公钥以及共享密钥的简单串联。
    - 所使用的算法标识在 [oqs-kem-info.md](https://github.com/open-quantum-safe/oqs-provider/blob/main/oqs-template/oqs-kem-info.md) 中有记录。
  - TLS 中的混合后量子/传统签名：
    - 对于 X.509 证书中的公钥和数字签名，请参阅下面关于 X.509 的项目。
    - 对于 X.509 证书之外的数字签名以及 TLS 1.3 握手中的数字签名，所使用的数据结构遵循与 X.509 证书相同的编码格式，即传统和后量子签名的简单串联。
    - 所使用的算法标识在 [oqs-sig-info.md](https://github.com/open-quantum-safe/oqs-provider/blob/main/oqs-template/oqs-sig-info.md) 中有记录。
- 对于 X.509：
  - 混合后量子/传统公钥和签名：
    - 所使用的数据结构遵循互联网草案 [Internet X.509 Public Key Infrastructure: Algorithm Identifiers for Dilithium](https://datatracker.ietf.org/doc/draft-ietf-lamps-dilithium-certificates/)，即传统和后量子组件的简单串联，采用纯二进制/OCTET_STRING 表示。
    - 所使用的算法标识（OIDs）在 [oqs-sig-info.md](https://github.com/open-quantum-safe/oqs-provider/blob/main/oqs-template/oqs-sig-info.md) 中有记录。
- 对于 PKCS#8：
  - 混合后量子/传统私钥：
    - 传统和后量子组件的简单串联，采用纯二进制/OCTET_STRING 表示。

另外值得注意的是，只有量子安全的 [签名算法](#signature-algorithms) 通过 PKCS#8 和 X.509 进行持久化。对于量子安全的 [KEM 算法](#kem-algorithms) 没有相应的编码器/解码器逻辑存在 — 也可参见 #194。