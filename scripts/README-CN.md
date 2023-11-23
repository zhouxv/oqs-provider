# 构建和测试支持脚本

此目录包含各种脚本，旨在简化 `oqsprovider` 的构建和测试。

## 构建

关键文件是 [fullbuild.sh](fullbuild.sh)，其选项在 [这里](https://github.com/open-quantum-safe/oqs-provider/blob/main/CONFIGURE.md#convenience-build-script-options) 文档化。

## 测试

### API 测试

`ctest` 驱动的代码包含在 [test 目录](https://github.com/open-quantum-safe/oqs-provider/tree/main/test) 中，用于对所有功能和已启用的算法进行 API 测试。

### 命令行测试

通过 [runtests.sh](runtests.sh) 脚本以 `openssl` 命令行指令测试所有功能和已启用的算法，其选项在 [这里](https://github.com/open-quantum-safe/oqs-provider/blob/main/CONFIGURE.md#convenience-build-script-options) 文档化。

### 发布测试

通过 [release-test.sh](release-test.sh) 脚本，可以在客户端/服务器设置中通过相应的 `openssl s_server/s_client` 命令运行所有可能的签名和 KEM 算法的完整矩阵，以测试所有功能和所有算法。为了成功运行此测试，需要安装带有 `xdist` 扩展的 `python3` 和 `pytest`，例如，通过 `sudo apt install python3 python3-pytest python3-pytest-xdist python3-psutil`。测试必须在主项目目录中执行，例如，`./scripts/release-test.sh`。为了正常运行，建议使用本地且最新的（发布）安装的 `openssl` 和 `liboqs`（例如，通过 `scripts/fulltest.sh` 构建）。
