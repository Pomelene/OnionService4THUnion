# 社团服务 JSON 接口 v2

入口文件为仓库根目录的 `webapp.json`，配套 `webapp.schema.json` 提供编辑器字段校验。客户端兼容旧版 v1 和本版 v2。升级到支持 v2 的客户端后，本文列出的目录、样式和操作均可只修改本仓库，不需要重建启动器。

每次启动首次确认进入服务时同步一次，后续读取 `.onion/services/thunion/webapp.json`。同一次运行中修改远端配置，需要重新启动客户端才能同步；同步失败使用上次有效缓存。

## 顶层字段

| 字段 | 必需 | 含义 |
| --- | --- | --- |
| `$schema` | 否 | 建议填 `./webapp.schema.json`，用于编辑器校验 |
| `schema_version` | 是 | 新配置填 `2` |
| `title` | 是 | 服务页面标题，最多 80 字符 |
| `default_category` | 否 | 首次显示的栏目 ID；省略时显示第一个可见栏目 |
| `categories` | 是 | 栏目数组，数组顺序就是界面顺序；允许为空，最多 24 栏 |

## 栏目字段

| 字段 | 必需 | 含义 |
| --- | --- | --- |
| `id` | 是 | 唯一标识，最多 64 字符；修改标题时建议保持 ID 不变 |
| `title` | 是 | 栏目名称，最多 80 字符 |
| `description` | 是 | 栏目用途，最多 400 字符 |
| `tools` | 是 | 条目数组；数组顺序就是显示顺序，每栏最多 32 项 |
| `layout` | 否 | `cards` 卡片（默认）或 `list` 紧凑列表 |
| `icon` | 否 | 左侧栏目图标，默认 `folder` |
| `visible` | 否 | `false` 暂时隐藏整栏，默认 `true` |
| `show_urls` | 否 | 是否显示网址全文，默认 `true`；不影响点击操作 |

## 条目字段

| 字段 | 必需 | 含义 |
| --- | --- | --- |
| `id` | 是 | 当前栏目内唯一，最多 64 字符 |
| `title` | 是 | 名称，最多 80 字符 |
| `description` | 是 | 说明用途，最多 800 字符 |
| `url` | 是 | 公网 HTTP(S) 网址；拒绝本机、局域网 IP、嵌入凭据和脚本协议 |
| `action` | 否 | `web` 内嵌网页（默认）、`external` 系统浏览器、`download` 整合包下载、`copy` 复制链接 |
| `button_text` | 否 | 自定义按钮文字，最多 32 字符；省略时按操作生成 |
| `icon` | 否 | 条目图标 |
| `badge` | 否 | 名称旁的简短标签，例如版本，最多 24 字符 |
| `visible` | 否 | `false` 暂时隐藏条目，默认 `true` |
| `show_urls` | 否 | 覆盖本栏的网址显示设置 |

`download` 还必须包含：

| 字段 | 含义 |
| --- | --- |
| `filename` | 保存文件名，仅支持 `.mrpack`，不能包含目录或 Windows 非法/保留名称 |
| `size` | 文件实际大小，单位字节，范围 1–268435456（256 MiB） |
| `sha256` | 文件 SHA-256，64 位十六进制字符 |

下载仅支持 HTTPS 直链，保存到启动器的 `.onion/downloads/`。校验通过后显示“导入整合包”操作；导入前再次校验。下载不会自动安装、执行程序或自动启动游戏。

可用图标：`folder`、`world`、`tools`、`package`、`download`、`upload`、`grid`、`image`、`schematic`、`home`、`java`、`skin`、`history`、`rocket`。

## 示例

```json
{
  "$schema": "./webapp.schema.json",
  "schema_version": 2,
  "title": "THUnion 社团服务",
  "default_category": "sharing",
  "categories": [
    {
      "id": "sharing",
      "title": "部员分享",
      "description": "部员分享的工具与资料。",
      "layout": "list",
      "icon": "tools",
      "show_urls": false,
      "tools": [
        {
          "id": "atlas-guide",
          "title": "Atlas 离线地图",
          "description": "打开后按网站指引下载。",
          "url": "https://atlas.thunion.net/guide.html",
          "action": "web",
          "button_text": "查看下载指引"
        }
      ]
    }
  ]
}
```

## 更新方式与边界

1. 编辑 `webapp.json`：新增/删除数组元素控制栏目和条目数量，移动元素控制顺序，修改 `layout` 和其他展示字段控制外观。
2. 上传或替换整合包时，同时更新直链、`size` 和 `sha256`。PowerShell 可用 `(Get-Item 文件名).Length` 及 `(Get-FileHash 文件名 -Algorithm SHA256).Hash` 获取后两项。
3. 提交到 `main`；客户端下次启动并确认进入服务时同步。

整份目录最多 256 KiB、128 个条目。客户端会额外检查重复 ID、默认栏目是否存在、下载文件名与网址安全性。文字按纯文本渲染；不支持下发 HTML、JavaScript、Python、命令行、任意组件代码或自动安装指令。新增本文之外的操作类型或布局仍需更新客户端；不能仅靠 JSON 注入新代码。

## 未推送的本地预览

将本仓库 `webapp.json` 复制为启动器目录下 `.onion/services/thunion/preview.json`，重启后首次确认进入服务。客户端会标明“本地预览”，本次运行只读该文件，不请求远端，也不会覆盖正常缓存。需要恢复远端同步时删除 `preview.json` 后重启。未发布的包可先复制到 `.onion/downloads/`，文件名须与配置一致；下载按钮会校验并复用缓存，导入仍需手动点击。此模式下不能据此判断尚未发布的 GitHub 直链是否可用。
