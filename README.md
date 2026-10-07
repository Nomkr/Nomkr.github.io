# 个人简历 / 学术主页

基于 [academicpages](https://github.com/academicpages/academicpages.github.io)（Jekyll + Minimal Mistakes 主题）搭建的个人主页，风格与 [GuangLun2000.github.io](https://guanglun2000.github.io/) 和 [xiaotan-git.github.io](https://xiaotan-git.github.io/) 一致。

当前包含三个页面：**Home（关于我）**、**Research（研究）**、**CV（简历）**。暂时没有 Publication 页面（以后有论文了可按第四节的说明加回）。

## 一、需要改哪些文件（重点）

| 文件 | 改什么 |
| --- | --- |
| `_config.yml` | 站点名、网址、侧边栏个人信息（名字 / 头像 / 简介 / 邮箱 / GitHub / 社交链接） |
| `_data/navigation.yml` | 顶部菜单（一般不用动） |
| `_pages/about.md` | 首页：自我介绍、研究方向、News |
| `_pages/research.md` | 研究项目 |
| `_pages/cv.md` | 教育经历、科研经历、荣誉、技能 |
| `images/profile.png` | 你的头像照片（正方形，替换这个文件即可） |

> 打开这几个文件，把所有 `[方括号]`、`Your Name`、`yourusername`、`XXX University` 之类的占位文字替换成你自己的信息即可。`_config.yml` 里带 `TODO` 注释的几行尤其要改。

## 二、本地预览（可选）

网站最终由 GitHub Pages 自动构建，你本地**不需要**装 Ruby。想本地预览的话任选其一：

1. **直接推到 GitHub 看线上效果**（最简单，见第三节）。
2. **VS Code + Docker DevContainer**（已装 Docker 的话推荐）：用 VS Code 打开本文件夹，按 `F1 → Dev Containers: Reopen in Container`，会自动装好环境并在 `http://localhost:4000` 实时预览。
3. **手动装 Ruby + Jekyll**：参见 [academicpages 官方文档](https://github.com/academicpages/academicpages.github.io#running-locally)。

## 三、部署到 GitHub Pages（上线）

1. 在 GitHub 新建一个仓库，名字必须是 **`<你的用户名>.github.io`**（例如 `zhangsan.github.io`）。
2. 把本目录的所有文件推到该仓库：
   ```bash
   git init
   git add .
   git commit -m "init resume site"
   git remote add origin https://github.com/<你的用户名>/<你的用户名>.github.io.git
   git push -u origin main
   ```
3. 打开仓库 **Settings → Pages → Build and deployment**，选 **Deploy from a branch**，分支 `main`、目录 `/ (root)`，保存。
4. 等 1~3 分钟构建完成，访问 `https://<你的用户名>.github.io` 即可。

> ⚠️ 部署前务必先把 `_config.yml` 里的 `url` 和 `repository` 改成你自己的（见文件里的 `TODO`），否则站内链接会错乱。

## 四、以后想加 Publication（可选）

1. `_config.yml` 的 `collections` 里已配置好 `publications`，无需再改。
2. 把每篇论文写成一个 `.md` 放进 `_publications/` 目录（可用 `markdown_generator/` 里的工具从 BibTeX 批量生成）。
3. 恢复 `_pages/publications.html` 页面，并在 `_data/navigation.yml` 里加一条：
   ```yaml
   - title: "Publications"
     url: /publications/
   ```

## 五、自定义配色

`_config.yml` 里的 `site_theme` 支持 `default / air / sunrise / mint / dirt / contrast` 六种配色，改一下即可。

---

模板来源：[academicpages/academicpages.github.io](https://github.com/academicpages/academicpages.github.io)（fork 自 Minimal Mistakes）。完整文档见其 [Wiki](https://github.com/academicpages/academicpages.github.io/wiki)。
