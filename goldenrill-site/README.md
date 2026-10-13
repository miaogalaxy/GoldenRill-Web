# Golden Rill 官网项目

一个面向投资机构的官网系统，包含**官网前端**（访客浏览的页面）和**管理后台**（后台维护内容的页面）两大部分。

> 说明：本项目目前是**单文件网页应用**。前端 `index.html` 和后端（管理后台）`admin.html` 都是自包含的单个 HTML 文件——图片、样式、JS 库全部内嵌在文件里，**不需要安装任何依赖、不需要数据库、不需要环境变量**。页面内容保存在浏览器的 `localStorage` 中，前后台在同一域名下共享这份数据。

---

## 一、项目结构

```
grpf-website/
├── frontend/                 # 官网前端（访客看到的页面）
│   └── index.html            #   网站主页面（7.7MB，单文件）
├── backend/                  # 管理后台（维护网站内容）
│   └── admin.html            #   后台登录 + 内容管理界面（4MB，单文件）
├── assets/                   # 素材库（Logo、图片、JS 库的原始文件，页面内未直接引用）
├── docs/                     # 文档与历史资料
│   ├── demo-pages/           #   历史演示页（卡片样式、背景效果、导航等试验版本）
│   ├── background-demo/      #   后台登录页背景效果演示（A–E 方案）
│   └── backup/               #   历史备份（旧版本文件、平台导出包）
└── README.md                 # 本说明文档
└── .gitignore                # Git 忽略规则
```

- **frontend/** = 给访客看的官网
- **backend/** = 给你自己用的管理后台（登录后可改文案、Logo、新闻等）
- 两边共用一份浏览器数据（`localStorage` 的 `grpf-admin-v6` 键），后台保存 → 前端自动同步。

---

## 二、本地如何启动（不用装任何东西）

因为项目是纯 HTML 单文件，**不需要安装依赖**，直接用浏览器打开即可：

### 方法 1：双击打开（最简单）
1. 打开文件管理器，进入 `frontend/` 文件夹
2. 双击 `index.html` → 浏览器打开官网前端
3. 进入 `backend/` 文件夹，双击 `admin.html` → 浏览器打开管理后台

> 注意：双击打开时地址栏是 `file://` 开头，前后台的数据互通可能受限（浏览器安全限制）。**推荐用方法 2**。

### 方法 2：本地起一个服务（推荐，前后台数据互通）
需要电脑上有 Python（Windows/Mac 一般自带；没有就去 python.org 装一个）：

1. 打开终端（Windows 按 `Win + R` 输入 `cmd` 回车；Mac 打开"终端"App）
2. 输入以下命令进入项目目录（把路径换成你的实际路径）：
   ```bash
   cd /你的路径/grpf-website
   ```
3. 启动本地服务：
   ```bash
   python3 -m http.server 8080
   ```
4. 浏览器打开：
   - 前端：`http://localhost:8080/frontend/index.html`
   - 后台：`http://localhost:8080/backend/admin.html`
5. 用完想关闭：在终端按 `Ctrl + C`

后台默认账号密码：`admin / admin123`（登录后可在后台修改全部网站内容）。

---

## 三、依赖与环境变量

| 项目 | 说明 |
|------|------|
| 运行依赖 | **无**。HTML、CSS、JS、图片全部内嵌在单文件里 |
| 数据库 | **无**。内容存浏览器 localStorage |
| 环境变量 | **无**。不需要配置任何环境变量 |
| 构建工具 | **无**。不需要 Node.js / npm / 打包 |

如果你以后把项目升级成"前后端分离 + 服务器数据库"架构（见第六节），届时才需要安装 Node.js、数据库等，并配置环境变量。

---

## 四、Git 版本管理（防止改坏、可随时回退）

> 先把项目放进 Git，之后每次改动都能留下记录，改坏了可以一键回到旧版本。

### 4.1 第一次初始化（只做一次）
1. 打开终端，进入项目根目录：
   ```bash
   cd /你的路径/grpf-website
   ```
2. 初始化 Git 仓库：
   ```bash
   git init
   ```
3. 把全部文件加入暂存区：
   ```bash
   git add .
   ```
4. 第一次提交（打上版本标签"v1"）：
   ```bash
   git commit -m "v1：官网前端 + 管理后台定稿"
   git tag v1.0
   ```

### 4.2 每次改完代码后保存一个版本（日常习惯）
```bash
git add .
git commit -m "改了什么内容（例如：更新首页文案）"
```

### 4.3 改坏了想回退到旧版本
```bash
# 查看历史版本
git log --oneline

# 把整个项目恢复到某个版本（把 1234567 换成 git log 里看到的版本号）
git checkout 1234567
```

### 4.4 推送到远程仓库（GitHub / Gitee / GitLab）
1. 在 GitHub（或 Gitee）上新建一个空仓库，复制它的地址（形如 `https://github.com/你的用户名/goldenrill-site.git`）
2. 在终端执行：
   ```bash
   git remote add origin https://github.com/你的用户名/goldenrill-site.git
   git push -u origin master
   ```
3. 以后每次提交后推送：
   ```bash
   git push
   ```

> `.gitignore` 已经配置好，`node_modules`、备份文件等不会传上去（详见文件内容）。

---

## 五、以后加功能怎么组织代码（扩展建议）

当前是单文件结构，适合维护简单的小网站。**以后功能变多时，按下面的方式组织，代码才不会乱**：

### 5.1 推荐目录（前后端分离 + 组件化）
```
goldenrill-site/
├── frontend/                     # 前端项目
│   ├── public/                   #   静态资源（图片、Logo）
│   └── src/
│       ├── components/           #   页面组件（每个板块一个文件）
│       │   ├── Navbar.vue        #     顶部导航
│       │   ├── Hero.vue          #     首页首屏
│       │   ├── Portfolio.vue     #     投资组合
│       │   ├── News.vue          #     新闻动态
│       │   └── Footer.vue        #     页脚
│       ├── views/                #   整页视图
│       ├── router/               #   页面路由
│       └── api/                  #   与后端接口的封装
├── backend/                      # 后端项目
│   ├── src/
│   │   ├── routes/               #   接口路由（按板块分文件）
│   │   ├── models/               #   数据模型（对应数据库表）
│   │   └── controllers/          #   业务逻辑
│   └── config/                   #   配置（数据库连接、环境变量）
└── database/                     # 数据库脚本
```

### 5.2 组织原则（简单 3 条）
1. **按功能分文件，不按文件大小分**：每个板块（导航/投资组合/新闻…）一个组件/一个路由/一个接口文件。
2. **前端只负责展示，后端只负责数据**：前端通过接口拿数据，不在页面里直接写死内容。
3. **数据放数据库，不放浏览器**：内容多、要多人协作时，把 localStorage 换成服务器数据库（MySQL/PostgreSQL）。

### 5.3 小步演进建议
- 第 1 步：把现在的两个大 HTML 文件拆成多个小文件（HTML + CSS + JS 分离）
- 第 2 步：引入 Vue/React 等框架，把每个板块做成组件
- 第 3 步：加一个真正的后端（如 Node.js/Express 或 Python/FastAPI），提供数据接口
- 第 4 步：加数据库，把 localStorage 迁移到数据库

---

## 六、部署到云服务器

### 6.1 当前版本怎么部署（纯静态，最简单）
当前是静态 HTML，任何能放网页的服务器都行：

**服务器配置建议（低配就够）**
| 项目 | 建议 |
|------|------|
| 服务器类型 | 云服务器 ECS / 轻量应用服务器 |
| 配置 | 1 核 CPU、1GB 内存、20GB 硬盘（约几十元/月） |
| 系统 | Ubuntu 22.04 LTS 或 Debian 12 |
| 带宽 | 1–3Mbps（访问量小足够；图片多可以买 5Mbps） |

**部署流程（5 步）**
1. 买一台云服务器（阿里云/腾讯云/AWS 等），系统选 Ubuntu
2. 用 SSH 登录服务器（Windows 用 PuTTY，Mac 终端直接 `ssh root@你的服务器IP`）
3. 安装 Nginx（网页服务器）：
   ```bash
   sudo apt update
   sudo apt install -y nginx
   ```
4. 把 `frontend/` 和 `backend/` 两个文件夹上传到服务器（可以用宝塔面板/FileZilla，或命令行 `scp`）
5. 配置 Nginx 指向网页目录：
   ```bash
   sudo nano /etc/nginx/sites-available/default
   ```
   把 `root` 改成你的网页目录（例如 `/var/www/goldenrill`），保存后：
   ```bash
   sudo systemctl reload nginx
   ```
   然后用浏览器访问 `http://你的服务器IP` 即可看到官网。

**绑定域名 + HTTPS**（可选但推荐）：在服务器商买一个域名，解析到服务器 IP；再安装 Certbot 免费证书（`sudo apt install certbot python3-certbot-nginx` 后执行 `sudo certbot --nginx` 自动配置）。

### 6.2 以后升级成"前后端分离"后怎么部署
| 组件 | 方案 |
|------|------|
| 前端 | 构建后由 Nginx 托管静态文件 |
| 后端 | Node.js（PM2 守护）或 Python（Gunicorn + Nginx 反代） |
| 数据库 | MySQL / PostgreSQL（服务器商托管或自装） |
| 服务器配置 | 2 核 4GB 起，视访问量升级 |

大致流程：装 Node.js → 上传后端代码 → `npm install` → `npm run build`（前端）→ 用 PM2 启动后端 → Nginx 把 `/api` 转发给后端、其余交给前端静态文件。

---

## 七、定稿数据已固化（重要）

本项目已于 2026-10-13 将**定稿后的全部内容**（首页文案、基金产品、基金方向、核心业务、关于我们、投资组合、新闻、联系我们、团队、页脚、主题颜色、导航等）固化进了代码：

- 前端 `frontend/index.html` 与后台 `backend/admin.html` 内各内置了一份**定稿数据种子**（localStorage 键 `grpf-admin-v6`）
- 任何人**首次打开**网站或后台时，会自动加载这份定稿数据，无需手动配置
- 你在后台后续做的修改仍会保存到自己的浏览器 localStorage 并覆盖显示（种子只在浏览器没有数据时生效，不会覆盖你之后的修改）

> 这意味着：把本项目部署到服务器后，新访客看到的就是定稿内容；Git 仓库里的代码就是定稿版本。

## 八、常见问题

- **后台改了内容，前端没变？** 前后台要在同一域名下打开（推荐用"方法 2"的 localhost 方式，或部署后用同一域名）。改完保存后，前端页面会自动刷新同步。
- **上传图片提示"空间不足"？** 图片存在浏览器 localStorage 里，容量有限（约 5MB）。上传前先压缩图片，或用下面的升级方案把图片存到服务器。
- **文件太大（7MB）？** 单文件内嵌了全部图片，属正常。后续重构时把图片移到服务器可显著减小。

---

*本 README 由豆包协助编写，更新时间：2026-10-13*
