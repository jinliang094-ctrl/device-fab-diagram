# 附录 S：SVG 图元积木库（device-fab-diagram 按需附录，拼装剖面用）

用法：复制积木代码，按【改】列出的参数改数值，像搭乐高一样拼成完整剖面。填色遵循主文件配色表；绘制顺序遵循主文件"后画盖先画"。每个积木的【错】是高频翻车点，拼完对照主文件自检条款复查。

## CMOS 组

### 1. 衬底叠层（衬底 + 外延）
```svg
<rect x="60" y="380" width="780" height="60" fill="#F5B17B" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="60" y="340" width="780" height="40" fill="#FFF59D" stroke="#2C3E50" stroke-width="1.2"/>
```
锚点：外延层左上角 (60,340)。改：x、width、两层 y 与厚度。错：衬底不是最厚的层。

### 2. 阱区（P-Well / N-Well）
```svg
<path d="M110,240 L400,240 L400,300 Q400,340 350,340 L160,340 Q110,340 110,300 Z"/>
```
锚点：顶边 (110,240)→(400,240)。改：顶边两个 x、表面 y=240、阱底 y=340；四角 Q 控制点跟随边缘外移。错：用折线代替贝塞尔；阱底尖角。

### 3. STI 浅槽
```svg
<polygon points="90,240 170,240 158,282 102,282"/>
```
锚点：顶边 (90,240)→(170,240)。改：顶边两点、槽深（282−240）。错：画成矩形；深度超过阱深（STI 是浅槽）。

### 4. 源漏扩散区（n⁺ / p⁺）
```svg
<polygon points="200,240 250,240 246,264 204,264"/>
```
锚点：顶边。改：四点坐标，深度约 24px。错：深度超过 STI；忘了换 n⁺绿/p⁺橙。

### 5. 栅 + 栅氧
```svg
<rect x="262" y="216" width="56" height="24" fill="#EC7063" stroke="#2C3E50" stroke-width="1.2"/>
<line x1="258" y1="241" x2="322" y2="241" stroke="#2C3E50" stroke-width="2.5"/>
```
锚点：栅左下角 (262,240)。改：x、y、width（栅长）。错：栅直接贴在硅上（必须有栅氧细线隔开）。

### 6. W 塞 / 接触孔塞
```svg
<polygon points="208,126 262,126 250,241 220,241"/>
```
锚点：底边中心 (235,241)。改：底中 cx、顶 y=126、底 y=241、顶半宽 27、底半宽 15（顶宽≈底宽×1.8）。错：上窄下宽画反；孔底悬空没落在目标层上，或穿透目标层。

### 7. 空接触孔（刻蚀后未填充）
```svg
<polygon points="208,126 262,126 250,241 220,241" fill="#fff" stroke="#2C3E50" stroke-width="1.2" stroke-dasharray="6,3"/>
```
参数同积木 6。错：与已填充塞子用同一填充，看不出"空"。

### 8. 金属条（M1/M2）
```svg
<rect x="180" y="96" width="230" height="30" fill="#85C1E9" stroke="#2C3E50" stroke-width="1.2"/>
```
锚点：左下角。改：x、y、width；必须盖住下方塞子顶边。错：金属比介质层还厚；金属与塞错位。

### 9. Cu 大马士革单元
```svg
<polygon points="600,140 680,140 672,220 608,220" fill="#F1948A" stroke="#4A6FA5" stroke-width="3"/>
```
锚点：顶边中心。改：四点坐标；粗描边（`#4A6FA5` 3px）即 Ta/TaN 阻挡层。错：Cu 无阻挡层直接嵌介质；双层大马士革时上下两段断开。

### 10. LOCOS 场氧 + 鸟嘴（左半，水平镜像得右半）
```svg
<path d="M40,170 L200,170 Q228,198 238,220 Q268,233 295,244 Q266,250 240,254 Q231,267 229,282 L40,282 Z"/>
```
锚点：场氧左上角 (40,170)；鸟嘴尖 (295,244)。改：顶边 y、表面 y≈244、下凹界面 y=282、鸟嘴尖位置。错：鸟嘴与场氧不同色（必须同材质一体）；界面画平不下凹。

### 11. 钝化层
```svg
<rect x="60" y="70" width="780" height="20" fill="#AF7AC5" stroke="#2C3E50" stroke-width="1.2" opacity="0.85"/>
```
改：覆盖范围与 y。错：钝化层画进器件内部（它在最顶上）。

### 12. 光刻胶掩膜（带窗口）
```svg
<rect x="60" y="100" width="120" height="14" fill="#BB8FCE" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="300" y="100" width="140" height="14" fill="#BB8FCE" stroke="#2C3E50" stroke-width="1.2"/>
```
改：两段 rect 的 x/width，间隙即窗口。错：窗口位置与下方要加工的结构对不齐。

## MEMS 组

### 13. KOH V 型槽
```svg
<polygon points="180,112 300,112 252,198 228,198" fill="#fff" stroke="#2C3E50" stroke-width="1.2" stroke-dasharray="6,3"/>
```
锚点：窗口顶边 (180,112)→(300,112)。改：窗口宽 wW、底宽 bW；槽深 = (wW−bW)/2 × 1.41（tan54.7°），示例 (120−24)/2×1.41≈68→底 y≈180。错：画成 45° 或垂直侧壁（那是 DRIE）。

### 14. DRIE 深槽
```svg
<rect x="300" y="112" width="26" height="150" fill="#fff" stroke="#2C3E50" stroke-width="1.2" stroke-dasharray="6,3"/>
```
锚点：槽顶中心。改：槽宽（窄）、槽深（远大于宽）。错：侧壁带斜度（与 KOH 混淆）。

### 15. 悬空梁 + 锚点 + 空腔
```svg
<rect x="60" y="170" width="380" height="18" fill="#EC7063" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="60" y="188" width="90" height="12" fill="#EC7063" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="350" y="188" width="90" height="12" fill="#EC7063" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="150" y="188" width="200" height="12" fill="#fff" stroke="#2C3E50" stroke-width="1.2" stroke-dasharray="6,3"/>
```
锚点：梁左上角 (60,170)。改：梁长、梁厚、锚点宽、空腔跨度。错：忘画空腔（没有空腔就没有"悬空"）；锚点与梁不同色。

### 16. 背腔 + 敏感膜
```svg
<rect x="60" y="100" width="380" height="180" fill="#F5B17B" stroke="#2C3E50" stroke-width="1.2"/>
<polygon points="120,280 360,280 249,180 151,180" fill="#fff" stroke="#2C3E50" stroke-width="1.2" stroke-dasharray="6,3"/>
<rect x="130" y="100" width="140" height="10" fill="#AF7AC5" stroke="#2C3E50" stroke-width="1.2"/>
```
锚点：衬底左上 (60,100)。改：背腔开口宽、膜宽、腔深（侧壁 55°：半宽差 = 腔深/1.41）。错：薄膜不醒目；腔侧壁角度不对。

### 17. 晶圆键合
```svg
<rect x="60" y="150" width="380" height="60" fill="#F5B17B" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="60" y="212" width="380" height="80" fill="#F5B17B" stroke="#2C3E50" stroke-width="1.2"/>
<line x1="60" y1="211" x2="440" y2="211" stroke="#2C3E50" stroke-width="2.5"/>
```
改：两片厚度与 y；界面加粗线 y=211。错：界面线与普通边界同粗（看不出是键合面）。

## 通用组（标注）

### 18. 引线标注
```svg
<text x="150" y="30" font-size="15" fill="#222">材料名</text>
<line x1="160" y1="36" x2="228" y2="120" stroke="#555" stroke-width="1"/>
```
改：文字、文字位置（顶部标注区）、线终点（落在目标边缘，不穿入图形内部）。

### 19. 尺寸括号
```svg
<line x1="235" y1="62" x2="385" y2="62" stroke="#1A237E" stroke-width="1.3"/>
<line x1="235" y1="56" x2="235" y2="68" stroke="#1A237E" stroke-width="1.3"/>
<line x1="385" y1="56" x2="385" y2="68" stroke="#1A237E" stroke-width="1.3"/>
<text x="245" y="50" font-size="14" fill="#1A237E">标注文字</text>
```
改：括号跨距两端 x、横线 y、文字。错：括号与所指结构水平对不齐。

### 20. 化学式与掺杂符号
```
下标：SiO<tspan baseline-shift="sub" font-size="70%">2</tspan>
上标：n<tspan baseline-shift="super" font-size="70%">+</tspan>
```