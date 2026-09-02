# CLAUDE.md

Zhangchi Ma (henrymzc) 的学术个人主页。Economist, job market candidate。
Repo: `henrymzc/henrymzc.github.io` — 通过 GitHub Pages 发布到 https://henrymzc.github.io

## 技术栈

AcademicPages 模板 (Jekyll 3.9 + Minimal Mistakes 主题)。

## 本地环境 —— 重要

**必须用 Ruby 3.2,不要升级到 3.4/4.x。**

GitHub Pages 锁死了 Jekyll 3.9 + Liquid 4.0.3。新版 Ruby 和它们不兼容,已经踩过两个坑:
- Ruby ≥3.4:`csv` 移出标准库 → `LoadError: cannot load such file -- csv`
- Ruby ≥3.2:`tainted?` 方法被删 → `undefined method 'tainted?'`(Liquid 4.0.3 还在调它)

本地环境必须镜像线上,所以固定用 brew 的 `ruby@3.2`。`~/.zshrc` 里的 PATH:

```bash
export PATH="/opt/homebrew/opt/ruby@3.2/bin:$PATH"
export PATH="/opt/homebrew/lib/ruby/gems/3.2.0/bin:$PATH"
```

验证:`ruby -v` 应显示 `3.2.x`。若显示别的版本,检查 `.zshrc` 里有没有残留指向其他 Ruby 的 export(注意 conda 的 `(base)` 环境也可能抢 PATH)。

**本地预览:**
```bash
cd henrymzc.github.io
bundle exec jekyll serve
# → http://localhost:4000
```
改 `.html`/`.md` 存盘后自动重新构建,刷新浏览器即可。改 `_config.yml` 要 Ctrl+C 重启。

## 文件约定

- 页面在 `_pages/`,导航项在 `_data/navigation.yml`
- **图片和 PDF 一律放根目录的 `/files/`**
- **路径必须用根路径(开头带斜杠)**:`/files/fig_jmp.png`,不能写 `files/fig_jmp.png`。因为页面 permalink 是 `/research/`,相对路径会被解析成 `/research/files/...` 而 404
- 文件名全小写、无空格(服务器区分大小写)

## research 页面 (`_pages/research.html`)

自定义的单页论文展示,**不用主题默认的 collection 结构**(经济学圈习惯所有论文在一页看完)。

**每篇论文的信息层级(固定顺序):**
标题 → 作者 → 期刊/状态 → 领域标签 → 链接组 → 可折叠摘要

**关键约定:**
- 作者行里自己的名字用 `<span class="me">Zhangchi Ma</span>` 加粗,其他人不加粗。**注意 span 必须闭合**,历史上多次因为漏 `</span>` 导致后面文字全变粗
- 每篇论文结构:`<article class="paper">` > `<div class="paper-row">` > `<div class="paper-main">` + `<div class="paper-fig">`。闭合顺序是 paper-fig → paper-row → article,**别搞反**
- 领域标签(`.tag`)按研究领域配色,同一关键词在所有论文里颜色一致。**目前全部注释掉了**,想显示就删掉外面的 `<!-- -->`
- 图片:右侧 130px 缩略图,悬停放大 3.2 倍。**没有真图的论文,把整个 `<div class="paper-fig">` 删掉**,否则显示破图标
- Publications 板块目前注释隐藏(还没有发表)

**MathJax 已引入**(页面底部)。写公式用 `$...$`,比如 `$\lambda$`。

**但文本格式必须用 HTML,不能用 LaTeX** —— MathJax 只处理数学模式内的内容:
- 加粗:`<strong>x</strong>` ✗ 不是 `\textbf{x}`
- 引号:`"x"` ✗ 不是 `` ``x'' ``
- 百分号:`%` ✗ 不是 `\%`
- 破折号:`—` ✗ 不是 `---`
- 行尾不要有 `\\`

## 当前待办

- [ ] JMP 的图 `fig_jmp.png` 放进 `/files/`
- [ ] "AI and Labor Market" 论文摘要还是占位文字
- [ ] 确认合作者姓名拼写(Xavier D'Haultfœuille)
- [ ] 决定领域标签要不要显示出来

## 上线

```bash
git add .
git commit -m "..."
git push
```
push 后 GitHub Pages 自动构建,约 1-2 分钟生效。**建议先本地 `jekyll serve` 确认再 push。**
