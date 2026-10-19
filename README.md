# GitHub Pages 部署步骤

这份手册对应项目里的 `docs/` 静态站点。推荐使用
`retail-analytics-github-pages.zip` 发布，体积小、上传快；完整项目可以另行备份。

## 0. 准备

- 一个 GitHub 账号：<https://github.com/>
- 浏览器：Chrome / Edge 均可
- 文件：`retail-analytics-github-pages.zip`（约 10.5 MB）

解压后，顶层应该是：

```text
index.html
assets/
downloads/
README.md
.nojekyll
```

如果看到的是 `retail-analytics-github-pages/index.html` 这种多一层目录的结构，
请把里面的文件全部上移一层，确保 `index.html` 直接位于仓库根目录。

## 1. 新建 GitHub 仓库

1. 登录 GitHub，点击右上角 `+ → New repository`。
2. Repository name 填 `online-retail-analytics`。
3. 选择 `Public`（GitHub Free 的 Pages 建议用公开仓库）。
4. 不要勾选 “Add a README file”，保持空仓库。
5. 点击 `Create repository`。

## 2. 上传网页版文件

1. 在仓库首页点击 `uploading an existing file`，或 `Add file → Upload files`。
2. 打开解压后的文件夹，全选里面的**内容**：
   `index.html`、`assets`、`downloads`、`README.md`、`.nojekyll`。
3. 拖到 GitHub 上传区域。
4. Commit message 填 `Add GitHub Pages portfolio`。
5. 点击 `Commit changes`。

注意：拖的是文件夹里的内容，不要把外层文件夹整个拖进去；否则首页会变成
`/online-retail-pages/index.html`，Pages 根目录会找不到 `index.html`。

## 3. 开启 GitHub Pages

1. 进入仓库的 `Settings`。
2. 左侧点击 `Pages`。
3. `Build and deployment → Source` 选择 `Deploy from a branch`。
4. Branch 选择 `main`。
5. Folder 选择 `/ (root)`。
6. 点击 `Save`。

等待 1–3 分钟，刷新 Pages 页面。顶部会出现：

```text
Your site is live at https://<你的用户名>.github.io/online-retail-analytics/
```

## 4. 验证

用无痕窗口打开上面的网址，依次检查：

- 首页能正常显示；
- 关键图表图片能加载；
- `下载 Excel 看板` 能下载 `.xlsx`；
- `阅读项目报告` 能在浏览器打开 PDF；
- `下载 Power BI 工程包` 能下载 `.zip`；
- `下载代码包` 能下载 SQL/Python 源码。

## 5. 更新网站

以后修改内容时：

1. 在仓库点击 `Add file → Upload files`。
2. 上传同名文件覆盖旧文件。
3. Commit 后等待 Pages 自动重新构建。

## 6. 常见问题

### 打开是 404

- 确认仓库 `Settings → Pages` 已保存；
- 确认 Branch 是 `main`，Folder 是 `/ (root)`；
- 确认仓库根目录直接有 `index.html`；
- 刚开启 Pages 时等待 1–3 分钟，或到 `Actions` 看构建是否完成。

### 图片不显示

确认 `assets/figures/` 和 `assets/preview/` 已上传，并且文件名大小写一致。

### 下载按钮 404

确认 `downloads/` 目录已上传，里面有：

```text
online-retail-dashboard.xlsx
online-retail-report.pdf
online-retail-pbip.zip
online-retail-code.zip
```

### 上传大文件失败

GitHub 网页上传对单个大文件有限制。本项目网页版最大文件约 8.7 MB，
正常不会失败；如果上传完整项目中的原始 Excel，建议改用 GitHub Desktop
或 Git 命令行。

## 7. 方案 A：上传完整项目

如果希望仓库里保留 SQL、Python、数据和处理过程：

1. 上传整个项目；
2. `Settings → Pages → main → /docs`；
3. 访问 `https://<用户名>.github.io/<仓库名>/`。

完整项目包含原始数据和 Parquet 文件，仓库会明显更大；网页版发布推荐方案 B。

## 8. 写进简历

发布成功后，复制网址，例如：

```text
https://yourname.github.io/online-retail-analytics/
```

在 Word/WPS 中选中“项目主页”或完整网址，按 `Ctrl + K` 粘贴链接。
同时保留一行纯文本网址，避免 ATS 系统丢失超链接。

## 说明

- 本目录是纯静态站点，不需要后端。
- Power BI 工程包是下载文件；GitHub Pages 不能直接运行 Power BI Desktop。
- 单个文件均小于 GitHub 100 MB 限制。
- `.nojekyll` 用于避免 Jekyll 处理带下划线/中文的静态文件。
