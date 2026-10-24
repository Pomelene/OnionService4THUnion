# THUnion 社团服务目录

供 Onion Minecraft Launcher THUnion 特供版读取的公开、免费服务配置。清华联盟工坊是非官方、非营利组织；此仓库不提供学校或社团身份认证。

客户端每次启动首次确认进入“工具坊 → 社团服务”时，同步本仓库 `main` 分支的 `webapp.json`，后续进入读取本地缓存。先展示工具目录、用途说明和网址；用户点击“打开”后才在启动器内加载网页。

## 修改目录

`schema_version` 当前为 `1`。`categories` 是子工具目录列表，每个目录包含唯一的 `id`、显示名称 `title`、用途说明 `description` 和 `tools` 列表。工具也包含 `id`、`title`、`description`，以及 HTTP(S) 网址 `url`。可增加多个目录，每个目录也可容纳多个工具。配置本身通过 GitHub HTTPS Contents API 的 raw 响应读取，避免分支 raw 地址的短时缓存。

当前实验入口：

- **社团 Wiki**：<http://wiki.thunion.club/>，浏览 Wiki 文档与知识条目。当前会重定向到站点维护者的 Wiki 地址。
- **在线地图**：<http://map.thunion.club/>，浏览服务器在线地图。
- **THUnion Atlas**：<https://atlas.thunion.net/>，第二个在线地图入口，网站同时提供离线导航模组下载。

配置只描述页面入口，不包含程序、脚本、安装命令、账号、令牌或密钥。客户端不会把启动器的微软登录令牌发送给这些网页。网站功能和访问限制由对应站点自行维护。

本仓库使用 MIT License。
