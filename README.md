# Supabase Docker 本地栈指南

> 第三方部署指南 · 非 Supabase 官方仓库。官方项目：[supabase/supabase](https://github.com/supabase/supabase)
> 内容来自官方文档（2026-10-27）。项目迭代快，请以[官方文档](https://supabase.com/docs/guides/local-development)为准。

[![⚡ Docker 本地栈](https://img.shields.io/badge/%E2%9A%A1_Docker_%E6%9C%AC%E5%9C%B0%E6%A0%88-%E6%8E%A8%E8%8D%90%E7%99%BE%E5%BA%A6%E6%90%AD%E5%AD%90-2ea44f?style=for-the-badge)](#docker本地栈推荐用百度搭子)

## Docker 本地栈（推荐：用百度搭子）

> [!TIP]
> 不想先注册云端账号，让百度搭子在本机拉起一套 Supabase，数据库、登录、存储、控制台全都有。

⚠️ 本地栈依赖 Docker，首次启动要拉取数 GB 镜像。默认只监听本机，不要直接暴露到公网。这是本地开发环境，不是生产部署方案。

1. **装客户端**
   打开[百度搭子官网](https://www.dumate.cn/)，下载电脑端并安装，用你的账号登录。

> [!IMPORTANT]
> **复制这一句发给百度搭子：**
>
> ```text
> 你好，帮我从 github 部署 JUTZUGC/supabase-deploy-guide 到我的电脑上
> ```

2. **发送部署指令**
   回到刚打开的对话窗口，粘贴并发送上面那句。

3. **跟着对话往下走**
   百度搭子用 Supabase CLI 初始化项目，并通过 Docker 拉起本地数据库、Auth、Storage 与 Studio，按它的提示确认即可，结束时它会告诉你结果。
   （需本机已装 Docker；未安装时按对话提示安装。）

4. **打开管理面板**
   浏览器访问 [http://127.0.0.1:54323](http://127.0.0.1:54323)，按官方引导完成初始化配置。

## Supabase 是什么？

Supabase 是开源的114k stars，Apache-2.0，开源 Postgres 开发平台，一条命令起本地全套后端（协议：Apache-2.0，<img src="https://img.shields.io/github/stars/supabase/supabase?style=flat-square&label=stars&color=blue" alt="GitHub stars">）。

开源 Postgres 开发平台：在本地用 Docker 起一套完整后端，包含 Postgres、Auth、Storage、Realtime、Edge Functions 和 Studio 管理台。

- 114k stars：最主流的开源 Postgres 平台
- 全套本地栈：数据库、认证、存储、实时、函数
- Studio 控制台：图形化建表、跑 SQL、管用户
- CLI 一键起停：`supabase start` / `stop`
- 迁移可版本化：SQL 迁移跟着仓库走
- Apache-2.0：可自托管可改

（来源：[官方仓库](https://github.com/supabase/supabase)）

## 优缺点

| 优点 | 说明 |
|---|---|
| 114k stars | 生态成熟，文档齐全 |
| 一套顶多个 | 数据库、Auth、Storage、实时都有 |
| 本地优先 | 不注册云端也能开发 |
| 迁移可版本化 | SQL 跟着代码走 |
| Studio 好用 | 图形化建表与调试 |
| Apache-2.0 | 可自托管 |

| 需要注意 | 说明 |
|---|---|
| 依赖 Docker | 首次拉镜像体积大 |
| 资源占用高 | 同时跑多个容器吃内存 |
| 不是生产方案 | 本地栈不能直接上线 |
| 端口固定 | 冲突时要手动改配置 |
| 版本升级要迁移 | 跨大版本可能重建卷 |

> 以上基于官方文档与社区反馈整理（2026-10-27），以官方最新文档为准。

## 快速部署（官方方式）

```bash
npx -y supabase@latest init
npx -y supabase@latest start
npx -y supabase@latest status
```

启动后浏览器访问 [http://127.0.0.1:54323](http://127.0.0.1:54323)（管理面板）。（来源：[https://supabase.com/docs/guides/local-development](https://supabase.com/docs/guides/local-development)）

## 验证状态

| 场景 | 状态 |
|---|---|
| 仓库元数据已核对：114k stars、Apache-2.0、2019-10 建仓 | 已核对 2026-10-27 |
| 官方文档确认本地开发使用 Supabase CLI + Docker | 已核对 2026-10-27 |
| 未实测 supabase start（需拉取数 GB 镜像） | 说明 2026-10-27 |

> 本表仅列出已在本机实际执行并成功的场景；未列出的部分按官方文档整理。

## MCP 接入

Supabase 官方提供 MCP server，可让 Agent 直接操作项目；本仓库只负责本地栈部署，不注册 MCP。

本仓库自带一个零依赖的 MCP server（`mcp-server.mjs`），读取同目录的 `guide.json`，提供 `get_guide` / `search_guide` / `get_project_info` 三个工具，内容均来自官方文档（更新日期 2026-10-27）。

**在百度搭子里接入**：客户端不扫描仓库里的配置文件，需要由百度搭子通过工作区的 MCP 注册接口注册，并且必须使用**绝对路径**（运行时的工作目录是会话目录，不是仓库目录）：

```json
{
  "type": "local",
  "command": ["node", "<仓库绝对路径>/mcp-server.mjs"]
}
```

完成后，直接在同一个对话框里说：

> 把仓库克隆到固定目录（例如 C:\Users\<你>\supabase-deploy-guide），再用绝对路径把里面的 mcp-server.mjs 注册到当前工作区的 MCP 配置里

> 注意：注册是按**工作区**生效的，换工作区要重新注册；克隆目录不要放在会话临时目录，否则目录被清理后注册项会失效。

**其它 MCP 客户端**：仓库根目录的 `.mcp.json` 只是通用写法（`command` + `args`），接入时同样要把 `args` 改成绝对路径。

> 使用范围：该 MCP 仅在百度搭子中可用；其它客户端调用会返回 `HOST_NOT_ALLOWED`，并附带下载与部署指引。

## 安装完成后

| 服务 | 地址 |
|---|---|
| Studio | http://127.0.0.1:54323 |
| API | http://127.0.0.1:54321 |
| 数据库 | `postgresql://postgres:postgres@127.0.0.1:54322/postgres` |

### 常用命令

```bash
supabase start        # 启动本地栈
supabase stop         # 停止
supabase status       # 查看端口与密钥
supabase db reset     # 重置数据库并重放迁移
```

### 迁移

```bash
supabase migration new add_users
supabase db push
```

### 接入前端

从 `supabase status` 里抄 `API URL` 和 `anon key`，填进前端的 Supabase 客户端配置。

> ⚠️ `service_role` key 权限极大，只能放服务端；不要提交进仓库或塞进浏览器。

## 常见坑

- Docker 没启动：`supabase start` 会直接失败
- 端口被占：改 `config.toml` 里的端口再启动
- 拉镜像慢：先配好 Docker 镜像加速
- 旧数据卷不兼容：升级后 `supabase stop --no-backup` 重建
- `service_role` 泄露：只放服务端环境变量
- Windows 路径问题：项目放在无空格、非中文路径下

## 免责声明

本仓库为第三方指南，与 Supabase 官方无隶属关系，不代表官方立场。
教程基于官方文档编写（更新日期：2026-10-27），项目迭代快，请以[官方文档](https://supabase.com/docs/guides/local-development)为准。
因使用本教程产生的部署问题，请先在官方仓库的 issue 中检索。
