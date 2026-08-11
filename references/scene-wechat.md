# Scene: 公众号排版（杂志编号风）

> 适用于将 OB 文档排版为公众号文章。产出可直接复制粘贴进微信公众号编辑器的 HTML。

---

## 触发场景

用户说"做公众号排版"、"公众号HTML"、"帮我排版到公众号"、"做分发"、"一鱼多吃"等。

---

## ⚠️ 微信编辑器限制（最重要）

微信公众号编辑器有严格的标签限制：

1. **只能用 `<section>` 标签** — `<div>` 的内联样式会被吃掉，格式丢失
2. **全内联样式** — 不能用 `<style>` 标签、CSS class、外部样式表
3. **不能用** `<figure>`、`<figcaption>`、`<article>`、`<main>` 等语义标签
4. **img 标签极简** — 只有 `src` + `style`，不加多余属性
5. **base64 版必须** — 微信编辑器粘贴时需要图片内嵌，否则无法抓取

---

## 📐 整体规格

| 属性 | 值 |
|------|-----|
| 最大宽度 | 677px |
| 页面外底色 | `#FFF8DE`（奶油米黄） |
| 正文阅读面 | `#FFFEFA`（暖白，仅作承载面） |
| 配色映射 | 原作者主强调色→旧勃艮第棕；黄色→酪乳黄；点缀色→柔雾蓝；浅底→奶油米黄 |
| 正文字号 | 18px |
| 行高 | line-height: 2 |
| 正文色 | `#2D2423`（墨色） |
| 字体栈 | `-apple-system, 'PingFang SC', 'Helvetica Neue', sans-serif` |
| 标签 | 全部用 `<section>`，禁止 `<div>` |

---

## 🏗️ 页面结构

```
body (background:#FFF8DE)
└── section (max-width:677px; margin:0 auto; background:#FFFEFA; padding:44px 26px 40px)
    ├── 蓝灰 kicker
    ├── 暖白留白标题区
    ├── 居中引言金句
    ├── 三主色装饰条
    ├── 奶油米黄引言区块
    ├── 章节 ×N（暖白阅读面 + 淡棕装饰词 + 酪乳黄短条）
    ├── 三主色分隔条（章节间）
    ├── ...
    ├── 结尾金句
    └── 签名档
```

---

## 🎨 组件样式

### 原作者结构 × IP 四色映射

- 保留原作者的克制排版：暖白长文阅读面、大量留白、少量浅底信息块，不给每个章节铺整块品牌色。
- 旧勃艮第棕 `#43302E` 替代原作者的蓝色主强调，用于主标题、英文装饰词、STEP 标签与核心识别。
- 酪乳黄 `#FFF1B5` 替代原作者的黄色，用于章节短条和荧光笔高亮。
- 柔雾蓝 `#C1DBE8` 替代原作者的红色点缀，只用于三主色装饰条和头像边框等小面积细节。
- 奶油米黄 `#FFF8DE` 替代原作者的浅黄色块，用于页面外底、导语框和图片占位。
- 暖白 `#FFFEFA` 是正文阅读承载面，不计入品牌四色比例。公众号长文优先保证阅读节奏，不机械按面积切分 50% / 25% / 15% / 10%。

### 蓝灰 kicker（顶部标签）
```html
<section style="text-align:center; margin-bottom:14px;">
  <span style="font-size:13px; font-weight:bold; letter-spacing:5px; color:#506F7D;">标签文字</span>
</section>
```

### 大标题（可选，用于长文叙事）

#### 标题断行协议（P0）

- **不允许浏览器自动决定标题在哪断行。** 中文标题若超过一行，必须在生成 HTML 时用 `<br>` 按完整语义单元显式断句，优先在标点、转折或短语边界断开。
- **英文单词、数字、产品名、专有名词不可拆。** 例如 `Expression`、`Qwen3.7`、`PostHog` 必须以整词呈现；禁止出现 `Expressio / n` 这类断词。
- 空间不足时按此顺序处理：调整标题结构或装饰词位置 → 缩小标题字号 → 在中文语义边界手动换行；**不能压缩字距、硬拆英文，或留下一个孤字/单字符独占一行。**
- 主标题默认两行以内。两行必须在视觉重量和句意上均衡；短引子可单独成行，例如 `AI 时代，<br>学文科/艺术的人到底有什么优势？`。
- 英文装饰词必须独占一行，用 `white-space:nowrap; word-break:keep-all; overflow-wrap:normal;` 防止断词。字号按词长收缩：10 个字母以内用 56–68px；11–14 个字母用 48–56px；15–18 个字母用 38–48px。超过 18 个字母时，优先换成更短的同义装饰词；绝不换行、断词或用压缩字距硬塞。

```html
<section style="text-align:center; margin-bottom:40px; padding:50px 0 40px;">
  <!-- 中文标题按语义手动断行；英文、数字、专有名词使用 nowrap span 保持完整 -->
  <p style="margin:0 0 12px; font-family:Georgia,'Songti SC',serif; font-size:32px; font-weight:900; color:#43302E; line-height:1.5; word-break:normal; overflow-wrap:normal;">AI 时代，<br>学文科/艺术的人到底有什么优势？</p>
  <p style="margin:0 0 24px; font-size:17px; color:#6B5955; line-height:1.8;">副标题</p>
</section>
```

### 引言金句（居中衬线）
```html
<section style="text-align:center; margin-bottom:10px;">
  <p style="margin:0; font-family:Georgia,'Songti SC',serif; font-size:21px; font-weight:900; line-height:1.9;">金句文字<br>第二行<span style="color:#43302E;">旧勃艮第棕关键词</span>。</p>
</section>
```

### 三主色装饰条
```html
<section style="text-align:center; margin-bottom:36px;">
  <span style="display:inline-block; width:36px; height:4px; background:#43302E; border-radius:2px;"></span>
  <span style="display:inline-block; width:18px; height:4px; background:#FFF1B5; border-radius:2px; margin-left:5px;"></span>
  <span style="display:inline-block; width:8px; height:4px; background:#C1DBE8; border-radius:2px; margin-left:5px;"></span>
</section>
```

### 引言区块（奶油米黄浅底）
```html
<section style="margin-bottom:36px; padding:24px 22px; background:#FFF8DE; border-radius:16px;">
  <p style="margin:0 0 14px; font-size:16px; line-height:2; color:#2D2423;">引言正文</p>
  <p style="margin:0; font-size:16px; line-height:2; color:#2D2423;">第二段</p>
</section>
```

### 章节头三件套

教程/步骤类用四件套（含 STEP 标签），叙事类用三件套（装饰词 + 标题 + 黄条）：

**叙事类（推荐）：**
```html
<section style="margin-bottom:52px;">
  <!-- 大淡色英文装饰词 -->
  <section style="margin-bottom:6px; overflow:visible;">
    <span style="display:inline-block; font-family:Georgia,'Songti SC',serif; font-style:italic; font-size:68px; font-weight:bold; color:rgba(67,48,46,0.14); line-height:1; white-space:nowrap; word-break:keep-all; overflow-wrap:normal;">EnglishWord</span>
  </section>
  <!-- 衬线标题 -->
  <section style="margin-bottom:10px;">
    <span style="font-family:Georgia,'Songti SC',serif; font-size:27px; font-weight:900;">中文章节标题</span>
  </section>
  <!-- 酪乳黄短条 -->
  <section style="margin-bottom:22px;">
    <span style="display:inline-block; width:56px; height:6px; background:#FFF1B5; border-radius:3px;"></span>
  </section>
  <!-- 正文内容 -->
  <p style="margin:0 0 18px; font-size:18px; line-height:2; color:#2D2423;">段落文字</p>
</section>
```

**教程/步骤类（四件套）：**
```html
<section style="margin-bottom:52px;">
  <section style="margin-bottom:6px; overflow:visible;">
    <span style="display:inline-block; font-family:Georgia,'Songti SC',serif; font-style:italic; font-size:68px; font-weight:bold; color:rgba(67,48,46,0.14); line-height:1; white-space:nowrap; word-break:keep-all; overflow-wrap:normal;">01</span>
  </section>
  <section style="margin-bottom:4px;">
    <span style="font-size:13px; font-weight:bold; letter-spacing:4px; color:#43302E;">STEP 1</span>
  </section>
  <section style="margin-bottom:10px;">
    <span style="font-family:Georgia,'Songti SC',serif; font-size:27px; font-weight:900;">标题</span>
  </section>
  <section style="margin-bottom:22px;">
    <span style="display:inline-block; width:56px; height:6px; background:#FFF1B5; border-radius:3px;"></span>
  </section>
  <!-- 内容 -->
</section>
```

### 图片
```html
<img src="图片路径或base64" style="width:100%; border-radius:14px; margin-bottom:20px;">
```

### 图片占位（模板中使用）
```html
<section style="width:100%; height:200px; border-radius:14px; margin-bottom:20px; background:#FFF8DE; display:flex; align-items:center; justify-content:center; position:relative; overflow:hidden;">
  <span style="font-family:Georgia,serif; font-size:120px; font-weight:bold; color:rgba(67,48,46,.08); position:absolute; top:50%; left:50%; transform:translate(-50%,-50%);">IMG</span>
  <span style="font-size:13px; color:#506F7D; position:relative; z-index:1;">配图位置</span>
</section>
```

### 图注（可选）
```html
<section style="text-align:center; margin-bottom:20px;">
  <span style="font-size:13px; color:#9A837D;">△ 图片说明文字</span>
</section>
```

### 荧光笔高亮（加粗文字）
```html
<span style="background:linear-gradient(transparent 60%, #FFF1B5 60%); font-weight:bold; padding:0 2px;">高亮文字</span>
```
每节 1-3 处，不贪多。对应源文档中 `**加粗**` 的文字。

### 三主色分隔条（章节之间）
```html
<section style="text-align:center; margin-bottom:56px;">
  <span style="display:inline-block; width:36px; height:4px; background:#43302E; border-radius:2px;"></span>
  <span style="display:inline-block; width:18px; height:4px; background:#FFF1B5; border-radius:2px; margin-left:5px;"></span>
  <span style="display:inline-block; width:8px; height:4px; background:#C1DBE8; border-radius:2px; margin-left:5px;"></span>
</section>
```

### 结尾金句
```html
<section style="text-align:center; margin:48px 0 36px;">
  <span style="font-family:Georgia,'Songti SC',serif; font-style:italic; font-size:56px; color:rgba(67,48,46,.18); line-height:1;">"</span>
  <p style="margin:8px 0 6px; font-family:Georgia,'Songti SC',serif; font-size:21px; font-weight:900; line-height:1.8;">核心金句文字</p>
  <p style="margin:0 0 16px; font-size:14px; color:#9A837D;">副句 / 补充</p>
</section>
```

### 签名档

```html
<section style="text-align:center; padding:20px 0 0;">
  <p style="margin:0 0 4px; font-size:15px; font-weight:bold; color:#43302E;">栗栗安</p>
  <p style="margin:0; font-size:13px; color:#6B5955; line-height:1.8;">▪️在AI时代认真生活的女生｜INTJ<br>▪️跟Agent搭档的第1年</p>
</section>
```

---

## 📝 排版原则

### 内容处理
- **文字 100% 使用原文**，不改写、不精简、不添加
- `**加粗**` → 荧光笔高亮 span
- `![[filename]]` → 对应图片的 img 标签
- `## 标题` → 章节头三件套
- `### 小标题` → 加粗 18px 段落
- `---` → 三主色分隔条
- `> 引用` → 奶油米黄浅底区块
- 普通段落 → `<p>` 标签

### 叙事长文的章节装饰词
为每个 `## 标题` 匹配一个英文装饰词（Georgia italic 68px 淡色），例如：
- 剧本 → Script
- 米兰 → Milan
- 最后 → Finale

装饰词要短（1-2个英文单词），跟章节主题相关。

---

## 🖼️ 图片处理（base64 版）

必须产出 base64 版（文件名加 `-base64` 后缀），用 Python PIL：

```python
from PIL import Image
import base64, io

def img_to_base64(path, max_width=1080, quality=72):
    img = Image.open(path)
    if img.width > max_width:
        ratio = max_width / img.width
        img = img.resize((max_width, int(img.height * ratio)), Image.LANCZOS)
    buffer = io.BytesIO()
    img.convert('RGB').save(buffer, format='JPEG', quality=quality)
    return base64.b64encode(buffer.getvalue()).decode()
```

- 宽度缩到 1080px
- JPEG quality=72（控制体积）
- PNG 也转 JPEG
- 目标整体文件 < 5MB

---

## ✅ Checklist

- [ ] 全部用 `<section>` 标签，0 个 `<div>`
- [ ] 全内联样式，无 `<style>` 标签
- [ ] 外部底色 `#FFF8DE`，正文阅读面 `#FFFEFA`，max-width 677px
- [ ] 保留原作者的暖白阅读面和留白，不给每个章节铺整块品牌色
- [ ] 四色按角色替换：勃艮第棕主强调、酪乳黄高亮、柔雾蓝点缀、奶油米黄浅底
- [ ] 正文 18px，line-height:2
- [ ] 章节头三件套完整（装饰词 + 标题 + 黄条）
- [ ] 加粗文字 → 荧光笔高亮
- [ ] 图片 width:100%; border-radius:14px
- [ ] 三主色分隔条在章节之间
- [ ] 有结尾金句 + 签名档
- [ ] base64 版已生成，图片内嵌可粘贴
- [ ] 文字 100% 原文未改写
- [ ] 主标题两行以内；若换行，已用 `<br>` 在中文语义边界显式断句
- [ ] 英文单词、数字、产品名和专有名词没有被断开；无孤字、单字符独占一行
- [ ] 英文装饰词完整独占一行；字号已按词长收缩，未强行挤压、换行或断词

---

## 📎 参考定稿

- 本仓库 `assets/demo-wechat.html`（完整 Demo，含所有组件示例）
- 本仓库 `assets/template-wechat.html`（可直接修改的模板骨架）
