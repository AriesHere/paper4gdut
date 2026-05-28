# Guangdong University of Technology Typst Template

A document template that complies with GDUT formatting standards, supporting both the undergraduate thesis and the programming course design report.

## Features

- **Two template types**:
  - Undergraduate thesis: includes Chinese/English abstracts, table of contents, body, references, acknowledgements, and appendix
  - Programming course design report: a concise course design report format
- Compliant with GDUT formatting specs: page layout, font sizes, heading hierarchy, line spacing, etc.
- Uses cover images from the official Word template (dotx)
- Easy-to-extend components: code blocks, equation numbering, etc.
- Clear chapter structure examples

## Quick Start

### 1. Install Typst

Refer to the [Typst official site](https://typst.app/) to install the CLI or use the online editor.

### 2. Clone or copy the template

Copy the template files into your project and ensure the directory structure is as follows:

```
paper4gdut/
├── template.typ      # core template
├── main.typ          # main thesis file
├── chapters/         # chapter directory
│   ├── 1-introduction.typ
│   ├── 2-related.typ
│   ├── 3-method.typ
│   ├── 4-experiment.typ
│   ├── 5-conclusion.typ
│   └── appendix.typ
└── images/           # image assets (extracted from dotx)
    ├── cover_logo.png    # emblem image
    └── cover_banner.png  # GDUT banner
```

### 3. Cover images

The template uses cover images extracted from the dotx document:

- `images/cover_logo.png` - emblem
- `images/cover_banner.png` - GDUT banner

To replace them, keep the file names the same, or pass custom paths via `--input` in `main.typ`:

```bash
typst compile --input logo-image=/path/to/logo.png --input banner-image=/path/to/banner.png main.typ
```

### 4. Configure fonts (optional)

If you need to use the template in an environment without preinstalled fonts, or want a specific font version, you can configure local fonts at the top of `main.typ`:

```typst
#import "template.typ": *
#import "template.typ": configure-fonts  // import configuration function

// enable local fonts (defaults to system fonts like SimSun, SimHei)
configure-fonts(
  songti: "/absolute/path/to/simsun.ttf",  // SimSun
  heiti: "/absolute/path/to/simhei.ttf",   // SimHei
  times: "/absolute/path/to/times.ttf",    // Times New Roman
  arial: "/absolute/path/to/arial.ttf",    // Arial (used for level-1 headings)
  code: "/absolute/path/to/jetbrainsmono.ttf", // code font
  enable: true
)
```

**Note**: If fonts are not configured or `enable: false`, the template uses default system font names (Windows defaults: SimSun, SimHei, Times New Roman, Arial).

### 5. Choose template type

Edit `main.typ` by commenting or uncommenting to choose between the **undergraduate thesis** or **programming course design report** template and add or remove chapter(s) if you need:

```typst
// ========== select templete type ==========
// uncommenting the templete you want to use

// Undergraduate thesis
#thesis(metadata, show_cover: true)[
  #include "chapters/1-introduction.typ"
  #include "chapters/2-related.typ"
  #include "chapters/3-method.typ"
  #include "chapters/4-experiment.typ"
  #include "chapters/5-conclusion.typ"
  #include "chapters/appendix.typ"
]

// Programming course design report
/*
#course-report(metadata)[
  #include "chapters/1-introduction.typ"
  #include "chapters/2-related.typ"
  #include "chapters/3-method.typ"
  #include "chapters/4-experiment.typ"
  #include "chapters/5-conclusion.typ"
]
*/
```

### 6. Fill in basic info

Edit `metadata/common.typ`:

```typst
#let metadata = (
  // ========== basic ==========
  title: "你的论文题目",           // 题目/课程设计题目
  title_en: "Your English Title", // 外文题目（仅论文需要）
  author: "姓名",                 // 作者姓名
  student_id: "学号",             // 学号
  advisor: "指导教师",            // 指导教师
  major: "专业",                  // 专业名称
  school: "学院",                 // 学院名称
  class_info: "20XX级 X班",       // 年级班别
  date: datetime.today(),

  // ========== 论文专用 ==========
  abstract_cn: [中文摘要内容...],     // 中文摘要
  keywords_cn: ("关键词1", "关键词2"),     // 中文关键词
  abstract_en: [English abstract...],     // 英文摘要（可选）
  keywords_en: ("keyword1", "keyword2"),     // 英文关键词（可选）

  // ========== 课程设计专用 ==========
  grade: "",           // 成绩（答辩后填写，可选）

  // ========== 通用 ==========
  header_text: none    // 页眉文字（默认使用题目）
  // 如果需要在页眉显示指定文字，请把上一行的 none 替换为 "页眉显示的文字"，即： header_text: "页眉显示的文字"
)
```

### 7. Write chapter content

Write each chapter under `chapters/` using standard Typst syntax. Common commands:

- Level-1 heading: `= 引言`
- Level-2 heading: `== 研究背景`
- Level-3 heading: `=== 国内外研究`
- Image: `#figure(image("path.jpg"), caption: [图片说明])`
- Table: use the `table` function
- Formula: `$E = mc^2$`
- Citation: `#cite(...)` (requires a bibliography setup)

### 8. Compile

```bash
typst compile main.typ
```

The output file is `main.pdf`.

## Template parameters

### `#thesis(...)` parameters (undergraduate thesis)

| Parameter | Type | Description |
|------|------|------|
| `title` | `string` | thesis title (required) |
| `title_en` | `string` | foreign title (optional) |
| `author` | `string` | author name (required) |
| `student_id` | `string` | student ID (required) |
| `advisor` | `string` | advisor (required) |
| `major` | `string` | major (required) |
| `school` | `string` | school/college (required) |
| `class_info` | `string` | class information (required) |
| `date` | `datetime` | submission date (defaults to today) |
| `abstract_cn` | `content` | Chinese abstract (required) |
| `keywords_cn` | `array` | Chinese keywords array (required) |
| `abstract_en` | `content` | English abstract (optional) |
| `keywords_en` | `array` | English keywords (optional) |
| `header_text` | `string \| none` | header text, defaults to thesis title |
| `show_cover` | `bool` | whether to show cover (default true) |
| `body` | `content` | main body content |

### `#course-report(...)` parameters (course design report)

| Parameter | Type | Description |
|------|------|------|
| `title` | `string` | course design title (required) |
| `author` | `string` | author name (required) |
| `student_id` | `string` | student ID (required) |
| `advisor` | `string` | advisor (required) |
| `major` | `string` | major (required) |
| `school` | `string` | school/college (required) |
| `class_info` | `string` | class information (required) |
| `grade` | `string` | grade (optional, after defense) |
| `date` | `datetime` | submission date (defaults to today) |
| `header_text` | `string \| none` | header text, defaults to title |
| `body` | `content` | main body content |

### Font configuration function

```typst
configure-fonts(
  songti: string,    // SimSun font path or name
  heiti: string,     // SimHei font path or name
  times: string,     // Times New Roman font path or name
  arial: string,     // Arial font path or name (used for level-1 headings)
  code: string,      // code font path or name
  enable: bool       // whether to enable local fonts, default true
)
```

**Example usage**:

```typst
// disable local fonts and use system defaults
configure-fonts(enable: false)

// specify font paths
configure-fonts(
  songti: "C:/Windows/Fonts/simsun.ttc",
  heiti: "C:/Windows/Fonts/simhei.ttf",
  arial: "C:/Windows/Fonts/arial.ttf",
  enable: true
)
```

## Formatting specifications

This template strictly follows the GDUT dotx template formatting specifications:

### Page setup

- Paper: A4
- Margins: top 30mm, bottom 25mm, left 30mm, right 20mm

### Body text fonts

- Chinese font: SimSun
- English font: Times New Roman
- Font size: Small Fourth (12pt)
- Line spacing: 1.5
- First-line indent: 2 characters

### Heading styles

- Level-1 heading: 16pt, Arial + SimHei, bold, centered, page break before
- Level-2 heading: 16pt, Arial + SimHei, bold
- Level-3 heading: 16pt, Arial + SimHei, bold
- Level-4 heading: 14pt, Arial + SimHei, bold

### Header and footer

- Header: 9pt, SimSun, centered, single line at bottom
- Footer: 9pt, Times New Roman, right-aligned page number

## Suggested directory structure

```
your-project/
├── main.typ          # main file
├── template.typ      # template (recommended to keep unchanged)
├── chapters/         # chapter files
│   ├── 0-abstract.typ    # abstract (optional)
│   ├── 1-introduction.typ
│   ├── 2-related.typ     # literature review / related technologies
│   ├── 3-method.typ      # methodology
│   ├── 4-experiment.typ  # experiments
│   ├── 5-conclusion.typ  # conclusion
│   └── appendix.typ      # appendix
├── images/           # image assets
│   ├── cover_logo.png    # emblem (extracted from dotx)
│   └── cover_banner.png  # banner
└── bibliography.bib  # references (optional)
```

## Notes

1. **Font issues**: Ensure Chinese fonts (SimSun, SimHei) and English fonts (Times New Roman, Arial) are installed. If you see missing font warnings during compilation, check the font names or use local font configuration.

2. **Image paths**: Use relative paths when referencing images in chapters; paths are relative to the directory containing `main.typ`.

3. **Cover images**: The template uses `images/cover_logo.png` and `images/cover_banner.png` extracted from the dotx file. To customize, replace these files or provide paths via `--input`.

4. **Headers and footers**: The header defaults to the thesis title and can be overridden via `header_text`. The footer is the page number in the bottom-right.

5. **Figure/table numbering**: Figures are automatically labeled as "图 X" and tables as "表 X".

6. **Code blocks**: Use `code_block(content, caption: "程序清单 X")` to render numbered code blocks.

## Customization

To modify styles, edit `template.typ`:

- Page margins: `set page(margin: ...)`
- Font size and line spacing: `set text(...)` and `set par(leading: ...)`
- Heading styles: `show heading.where(level: N): ...`
- Header and footer content: modify `header` and `footer` configurations

## FAQ

**Q: The compiler says it can't find fonts**

A: Use the local font configuration function and specify valid font file paths, or make sure SimSun, SimHei, Times New Roman, Arial, etc. are installed.

**Q: How do I add references?**

A: Use BibTeX. Typst supports `.bib` files and you can cite them via `#bibliography("bibliography.bib")`.

**Q: How do I adjust line spacing?**

A: Find `set par(leading: 1.5em, ...)` in `template.typ` and modify `leading`.

**Q: How do I add new chapters?**

A: Create a new file under `chapters/` (e.g., `6-future.typ`) and add `#include "chapters/6-future.typ"` in the template block of `main.typ`.

**Q: The cover images do not show up**

A: Ensure that `cover_logo.png` and `cover_banner.png` exist under `images/`, or pass the correct image paths via `--input`.

## License

MIT License

## Contributing

Issues and pull requests are welcome.

---

Good luck with your thesis!
