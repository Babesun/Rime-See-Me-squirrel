# Rime 西米 for Squirrel

> Rime 鼠须管（Squirrel）配色主题与外观即时预览生成器

fork 自 https://github.com/GJRobert/Rime-See-Me-squirrel

---

在原有配色功能的基础上增加了自动配色和布局预览的功能，代码主要是Vibe Coding的，可能有些细节还不完善，欢迎反馈。

---

## 主要功能

### 1. 自动配色（依主色生成完整主题）
- 输入**主色**（6 位十六进制色值，例如 `BD8F13`），即时生成主题颜色配置
- 支持**深浅模式**切换：自动 / 浅色 / 深色
- 基于 HSL 色彩空间 + WCAG 2.0 对比度算法
  - 文字与背景保证 ≥ 4.5:1 对比度（无障碍阅读）
  - 高亮区文字与主色背景保证 ≥ 4.5:1 对比度
  - 高亮区的 Comment / Label 通过与主色混合建立层级（20% / 40%）
- 自动根据主色亮度决定背景基调（亮主色 → 浅色背景；暗主色 → 深色背景）

### 2. 布局与方向
- **候选项布局**：`stacked`（堆叠式，垂直排列）/ `linear`（线性式，水平排列）
  - 线性模式 `#box` 自适应宽度（`fit-content`），跟随候选词长度
- **文字方向**：`horizontal`（水平）/ `vertical`（垂直，使用 `writing-mode: vertical-rl`）
- 两者组合会统一应用 `#box` / `#display` 样式（含高度、宽度、flex 设置）

### 3. 预编辑与候选嵌入
- **行内预编辑（`inline_preedit`）**：
  - 开启：组字区置于 `#box` 上方，默认开启
  - 关闭：组字区 `#select` 移入 `#box` 内，与候选项同一容器
- **候选嵌入（`inline_candidate`）**：
  - 开启：组字区的 `#hilited` 显示候选词汉字
  - 关闭：显示原始拼音

### 4. 视觉样式
- 圆角：`corner_radius`（背景框）、`hilited_corner_radius`（高亮项）
- 边框：`border_height` / `border_width` / `border_color`
  - 模拟 Squirrel 行为：值为 `-2` 表示不显示边框
  - 其他值渲染 `max(1, value + 1)` px（Squirrel 默认加 1px）
- 透明度：`translucency`（启用）+ `alpha`（0~1 数值）
- 阴影：`shadow_size`（>0 为高亮项加黑色阴影）
- 候选项缩放：`surrounding_extra_expansion`（自动计算其他候选项宽度）

### 5. 间距
- **`line_spacing`** 候选项边距：仅在 `stacked` 模式生效（线性模式自动清除）
- **`spacing`** 编码区边距：根据 `inline_preedit` 自动加在 `#display` 顶部或 `#box` 顶部

### 6. 字体设定（分别独立）
- 全局字体 / 候选字体：字体（`font_face`）、字号（`font_point`）
- 标签字体：字体（`label_font_face`）、字号（`label_font_point`）
- 注释字体：字体（`comment_font_face`）、字号（`comment_font_point`）

### 7. 候选格式自订
- `candidate_format`：使用 `[label]`、`[candidate]`、`[comment]` 三个占位符
- 默认 `[label]. [candidate] [comment]`，可自由组合

### 8. 文本微调
- `base_offset`：候选框垂直位移（`translateY`）
- `memorize_size`：紧贴屏幕边缘

### 9. 环境参考
- **环境背景色**：模拟深浅环境下的实际显示效果，也可手工修改颜色
- **BGR ↔ RGB 转换**：辅助工具，处理常见的色码位元顺序问题

### 10. 即时预览
- 中栏为**完整鼠须管 UI 预览**，所有设定变更即时反映
- 候选数滑杆（1~9）动态控制预览显示的候选项数量
- 颜色输入框背景自动反衬为黑/白以保证可读

### 11. 载入现有 YAML 主题（原功能，未做优化）
页面最下方可选择本机 YAML 主题文件并载入，自动：
- 解析配色方案名称、作者信息
- 套用所有色码到右栏输入框
- 套用所有非颜色属性（布局、方向、字体、边距等）到预览
- 重新触发边框计算（载入后边框宽/高 + 颜色会合成完整样式）

**YAML 主题文件格式**（每行前需空 2 格）：

```yaml
#Rime theme ← 此行非必要
color_scheme_example:    # 文件内只能有这个顶层对象
  name: 范例主题
  author: 作者名 <email@example.com>
  back_color: '0xECE2F2'              # ★ 限于目前程序功能，每个色码前后务必加 ' ' 才能以字符串格式读入
  border_color: '0x573270'
  text_color: '0x32705A'
  hilited_text_color: '0x705432'
  hilited_back_color: '0x7DC4AB'
  hilited_candidate_text_color: '0xF1EBF6'
  hilited_candidate_back_color: '0x573270'
  hilited_candidate_label_color: '0x7DC4AB'
  hilited_comment_text_color: '0xF6F1EB'
  candidate_text_color: '0x327051'
  comment_text_color: '0x705432'
  label_color: '0x000201'
```

### 12. 即时生成设置码
右栏显示完整的 Rime YAML 设置码，所有字段即时同步更新，可直接复制使用。
