# THUnion 社团服务目录

供 Onion Minecraft Launcher THUnion 特供版读取的公开、免费服务配置。清华联盟工坊是非官方、非营利组织；此仓库不提供学校或社团身份认证。

客户端每次启动首次确认进入“工具坊 → 社团服务”时，同步本仓库 `main` 分支的 `webapp.json`，后续进入读取本地缓存。先展示工具目录和用途说明，再按 JSON 配置提供内嵌网页、系统浏览器、复制链接或整合包下载。栏目与条目顺序完全由数组顺序决定。

## 修改目录

`schema_version` 当前为 `2`，客户端兼容旧版 v1。完整字段、布局和操作定义见 [API.md](API.md)，编辑器校验使用 [webapp.schema.json](webapp.schema.json)。`categories` 是子工具目录列表，每个目录包含唯一的 `id`、显示名称 `title`、用途说明 `description` 和 `tools` 列表。工具也包含 `id`、`title`、`description`，以及 HTTP(S) 网址 `url`。可增加多个目录，每个目录也可容纳多个工具。配置本身通过 GitHub HTTPS Contents API 的 raw 响应读取，避免分支 raw 地址的短时缓存。

其他服务入口：

- **社团 Wiki**：<http://wiki.thunion.club/>，浏览 Wiki 文档与知识条目。当前会重定向到站点维护者的 Wiki 地址。
- **在线地图**：<http://map.thunion.club/>，浏览服务器在线地图。
- **THUnion Atlas**：<https://atlas.thunion.net/>，第二个在线地图入口，网站同时提供离线导航模组下载。

配置只描述页面入口，不包含程序、脚本、安装命令、账号、令牌或密钥。客户端不会把启动器的微软登录令牌发送给这些网页。网站功能和访问限制由对应站点自行维护。

本仓库使用 MIT License。

## 部员分享

“部员分享”置于服务目录首位，包含：

- **[三周目快速上服基础包](https://raw.githubusercontent.com/Pomelene/OnionService4THUnion/main/263THUnionBasic.mrpack)**：`263THUnionBasic.mrpack`，Minecraft 26.3 / Fabric 0.19.5，由本仓库直接提供下载。
- **[Atlas 离线地图下载指引](https://atlas.thunion.net/guide.html)**：打开后按网页说明下载。
- **[Litematica-Flex](https://github.com/Pomelene/Litematica-Flex)**。
- **[Pomelene's Notebot](https://github.com/Pomelene/Pomelene-s-Notebot)**。

整合包大小与 SHA-256 摘要写入 `webapp.json`。支持下载条目的客户端先下载并校验文件，再提供用户主动选择的导入入口。下载条目使用 `action: "download"` 和 `filename` / `size` / `sha256`，不包含自动安装命令。
