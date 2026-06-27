这是一个将 **ShadowMountPlus**、**Garlic SaveMgr** 和 **ftpsrv** 合并到同一个 PS5 payload 中的构建版本。

合并版启动后，ShadowMountPlus 作为主进程运行，并在启动阶段尝试拉起两个嵌入模块：

- `garlic_savemgr_embedded_start()`：启动 Garlic SaveMgr Web 管理端。
- `ftpsrv_embedded_start()`：启动嵌入式 FTP 服务。

如果某个模块没有被链接进 ELF，ShadowMountPlus 会在日志里记录 `embedded module not linked`，不会阻塞主功能。

### 主要功能

- ShadowMountPlus 自动扫描、挂载和安装游戏镜像。
- 支持 exFAT / UFS / PFS / LVD / MD 等 ShadowMountPlus 原有能力。
- Garlic SaveMgr Web 端用于存档浏览、解密、加密、重签、导入、导出和云备份。
- ftpsrv 提供 FTP 访问，默认端口为 `2121`。
- Garlic SaveMgr Web 端默认端口为 `8082`。
- 合并版 ftpsrv 已禁用 `KILL` 命令，避免 FTP 客户端误关闭整个 ShadowMountPlus 进程。
- Garlic Web UI 已补充中文汉化，设置页、云备份、导入/导出、右键菜单、批量操作等主要可见文字已中文化。
Web 端主要页面：

- **浏览**：查看当前用户的 PS5 / PS4 存档。
- **解密**：上传加密存档并下载解密 ZIP。
- **加密**：上传解密后的文件夹并重新生成加密存档。
- **重签**：修改存档 Account ID。
- **导入**：导入解密文件夹或加密存档到系统。
- **设置**：配置 FTP / Google Drive / 本地 / USB 备份目标。

批量操作：

- 可按游戏勾选。
- 支持批量导出解密存档。
- 支持批量导出加密存档。
- 支持批量云备份。
### 日志查看

三个模块的运行日志统一写入：

```text
/data/sgfcnlog/
```

文件说明：

```text
/data/sgfcnlog/shadowmountplus.log    ShadowMountPlus 主程序日志
/data/sgfcnlog/shadowmountplus.log.1  上一次 ShadowMountPlus 日志
/data/sgfcnlog/garlic.log             Garlic SaveMgr 后端日志
/data/sgfcnlog/ftpsrv.log             ftpsrv 启动、网卡、监听和错误日志
```

ShadowMountPlus 的文件日志需要 `/data/shadowmount/config.ini` 中开启：
目前问题： 部分汉化 弹窗可能不弹 
