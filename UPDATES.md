# 启动器版本接口

`updates.json` 供 Onion 启动器公众版和 THUnion 版读取，与社团服务目录 `webapp.json` 独立。编辑器可使用 `updates.schema.json` 校验格式。

## 字段

| 字段 | 含义 |
| --- | --- |
| `schema_version` | 当前固定为 `1` |
| `releases` | 版本列表，数组先后顺序不影响版本选择 |
| `series` | `public`（公众版）或 `thunion`（特供版） |
| `version` | 显示版本，不带 `v`；特供版必须以 `-T` 结尾 |
| `order` | 同系列单调递增的整数，负责排序，不能重复 |
| `platform` | 当前为 `windows-x64` |
| `url` | 启动器 GitHub 仓库中对应版本 Release 的 EXE 下载链接 |
| `size` | EXE 的实际字节数 |
| `sha256` | EXE 的 SHA256 摘要，64 位十六进制 |
| `notes` | 可选纯文本更新说明 |

客户端从自身版本判断所属系列，仅当同系列最大 `order` 大于本地排序且版本号不同才提示。公众版不会被引导到特供版，反之亦然。可只保留两个系列各自最新版，也可保留历史记录。

## 发布步骤

1. 在客户端源码中更新版本号、Windows 版本资源和 `LAUNCHER_UPDATE_ORDER`。本表中初始 `0.3.2` / `0.3.2-T` 的排序均为 `32`，后续版本须增加排序。
2. 构建并测试，先上传可用的 GitHub Release EXE。
3. 使用该文件的真实下载地址、字节数和 SHA256 修改表格。`url` 中的版本标签必须与 `version` 一致，例如 `version: "0.3.3-T"` 对应 `/releases/download/v0.3.3-T/…exe`。
4. 检查 `public` 与 `thunion` 系列字段和文件对应关系，再提交本仓库。不要写入尚未上传的链接或占位校验值。

客户端启动检查失败时静默跳过；成功发现更新后询问用户，确认才下载。下载和替换前均校验 SHA256。更新器仅接受本启动器仓库的 HTTPS Release EXE；JSON 不支持脚本、命令或本地路径。

该接口由包含更新器的新客户端使用。已经发布的旧版 v0.3.2 EXE 没有自动更新代码，需要先手动升级一次。
