# KOJINZO — 故人像（こじんぞう）官网

一枚のお写真から故人像を制作するサービスの、日本語 1 ページサイト。ビルド不要の静的サイト（HTML + CSS + JS を 1 ファイルに内包）。

---

## 1. 文件结构

```
omokage-site/            ← 仓库根目录（Vercel 的 Root Directory 指向这里）
├── index.html           ← 全站内容（内嵌 CSS / JS，无外部依赖）
├── assets/
│   ├── logo-mark.png    品牌符号（页头 / 页脚，透明背景）
│   ├── favicon.png      站点图标（512×512，浏览器标签与 iOS 主屏）
│   ├── hero-sakura.jpg  首屏 樱花和纸
│   ├── craft-box.jpg    桐箱
│   ├── frame.jpg        （目前未被引用，可留作备用）
│   └── restore-desk.jpg （目前未被引用，可留作备用）
├── vercel.json          缓存与安全响应头
├── robots.txt
├── .gitignore
└── README.md
```

字体走 Google Fonts（Shippori Mincho / Cormorant Garamond / Noto Sans JP），需要联网；图片全部本地。

---

## 2. 本地预览

直接双击 `index.html` 即可。若要模拟线上环境（http 协议、相对路径）：

```bash
cd omokage-site
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000
```

---

## 3. 部署到 Vercel

### 方式 A：网页上传（不需要命令行）

1. 打开 https://github.com/new，Repository name 填 `kojinzo`，设为 **Private** 或 Public 均可（Vercel 两者都能连），不要勾选 README / .gitignore。
2. 进入新建的空仓库，点 **uploading an existing file**，把 `omokage-site` 文件夹**里面的全部内容**拖进去（`index.html`、`assets/`、`vercel.json`、`robots.txt`、`.gitignore`、`README.md`）。
   - 注意：.gitignore 是隐藏文件，拖拽可能被跳过，影响不大，可以不要。
3. Commit changes。
4. 打开 https://vercel.com/new → 选 **Import Git Repository** → 授权 GitHub → 选中 `kojinzo`。
5. 项目设置保持默认即可：
   - **Framework Preset**：`Other`
   - **Build Command**：留空
   - **Output Directory**：留空
   - **Root Directory**：留空（若把站点放在仓库子目录里，这里填该子目录名）
6. 点 **Deploy**，约 20 秒后拿到 `https://kojinzo-xxxx.vercel.app`。

### 方式 B：命令行

```bash
cd omokage-site
git init
git add .
git commit -m "Initial commit: KOJINZO site"
git branch -M main
git remote add origin git@github.com:<你的账号>/kojinzo.git
git push -u origin main
```

然后同上的第 4～6 步导入 Vercel。

### 之后的更新

改完文件后 `git add . && git commit -m "update" && git push`，Vercel 会自动重新部署（每次 push 都会生成一个 Preview 部署，main 分支才是正式环境）。

---

## 4. 自定义域名

Vercel 项目 → **Settings → Domains → Add**，输入域名后按提示到域名商改 DNS：

- 根域名 `kojinzo.jp` → A 记录指向 `76.76.21.21`
- `www` 子域名 → CNAME 指向 `cname.vercel-dns.com`

HTTPS 证书 Vercel 自动签发，不用手动配置。

---

## 5. 上线前待办（重要）

| # | 事项 | 说明 |
|---|---|---|
| 1 | **og:url / og:image 换成正式域名** | `index.html` 第 9 行附近有注释掉的两行 meta，把 `kojinzo.jp` 换成正式域名后取消注释。不做的话，LINE / X 分享不会出缩略图。 |
| 2 | **表单目前不发信** | 现在提交只弹「送信ありがとうございます」，数据不会到任何地方。需要接 Formspree（最省事）或 Vercel Serverless Function + 邮件服务。 |
| 3 | **换成真实作品照片** | 站内 4 张图为 AI 生成的氛围图。文案写了「掲載する作例はすべて実作」，上线前必须配人形成品照、修复前后对比，否则文案与内容互相矛盾。 |
| 4 | **会社概要・特定商取引法に基づく表記** | 日本接定制订单收款必须有。等运营主体（桜日和 or 合作方）确定后补。 |
| 5 | **价格复核** | 四档价格（¥108,000 / ¥128,000 / ¥218,000 / ¥328,000）为初期设定，确认成本后如有调整，改 `#price` 区块即可。 |
| 6 | assets 里两张未使用的图 | `frame.jpg`、`restore-desk.jpg` 当前没有任何引用，可删除以减小仓库体积。 |

---

## 6. 内容改动位置速查

| 想改什么 | 在 index.html 里找 |
|---|---|
| 品牌名 KOJINZO | 共 6 处：title / og:title / header / footer / copyright |
| 四档价格 | 搜 `class="plan` |
| 制作工期与每步时间 | 搜 `s-time` |
| 常见问题 | 搜 `faq-item` |
| 邮箱 | 搜 `li.shinoame@outlook.com` |
| 配色 | `:root` 里的 `--paper` `--ink` `--gold` |
