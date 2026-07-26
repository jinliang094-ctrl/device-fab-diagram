---
name: device-fab-diagram
description: 半导体器件工艺图绘制规范。当用户要求绘制半导体器件剖面图（cross-section/截面图）、多步制备流程剖面序列、工艺流程框图，涉及 CMOS、MEMS 等器件结构时触发。典型触发语：画剖面图、画器件图、画工艺流程、截面示意图、制备流程图，或用户显式调用本 skill。本 skill 一旦被触发或调用，在当前对话内持续生效直到对话结束：不仅响应用户的绘图请求，在讲解涉及器件结构、工艺流程、材料对比等内容时也必须主动按本规范配图，无需用户开口要求。
---

# 半导体器件工艺图绘制

直写 SVG，经 `show_widget` 输出自包含 HTML（内联 SVG），对话内即时渲染。目标：**快**（一次直出）、**准**（结构正确、标注齐全）。

## 铁律

1. 只用直写 SVG。禁用 ASCII 字符图、matplotlib、AI 生图、Mermaid。
2. **标注硬性**：每种材料、每个结构必须有文字（区内写或引线引出）；颜色只是辅助。
3. **模板优先**：能套模板绝不从零构图。CMOS 剖面一律从文末附录 A 模板改写；画 MEMS / 工艺框图前必须先读取对应附录文件（链接见文末"按需附录"）。
4. 一次直出：不复述需求、不追问细节；信息不足按最典型教科书结构画，画完用户自会纠正。
5. **直接出精修版**：结构、配色、标注、质感细节一次到位，不分标准版/细化版；用户说"从简"才减配。
6. **持续生效 + 主动配图**：本对话所有绘图按本规范执行；讲解涉及器件结构、工艺步骤、材料对比时主动配图，用户说"不用画"才停。

## 工作流程

1. 定图型：单张剖面 / 多步剖面序列（见附录 A 布局）/ 工艺流程框图（先读附录 C）。
2. 心里列结构清单：层叠顺序（自下而上）或工序顺序 + 要标注的材料名。不写出来。
3. 套模板改写：增删层、改标注。
4. 输出：白底 HTML + 内联 SVG。**SVG 内不写注释**；相同样式用 `<g>` 统一给属性；重复图元用 `<use>` 引用——省 token 也降出错率。
5. 用户说"细化 / 加一层 XX / 改成 XX"：只改对应部分，重出整图。

## 剖面画法

**构图**：viewBox 按需（单图典型 `0 0 800 450`），白底，结构居中，顶部留 15%~20% 作引线标注区。绘制顺序（后画盖先画）：衬底→外延→阱→STI→扩散区→ILD→栅→塞→金属→标注。边界 `stroke="#2C3E50" stroke-width="1.2"`。示意不按真实比例，但衬底最厚、阱次之、介质中等、金属薄。

**关键图元**：
- 阱：边界用 `path` 的 `Q` 贝塞尔平滑曲线，阱底平缓弧形，不用折线。
- STI：表面浅梯形槽，上宽下窄。
- 接触孔/塞：梯形，**上宽下窄**（顶宽≈底宽×1.8，左右对称收），孔底落在目标层上表面。
- 金属：薄矩形，与塞相接。空腔/Void：黑 `fill="#000"` 或白+虚线描边。MEMS 空腔一律白+虚线。

**标注**：
- 空间够写区内；不够引线（`stroke="#555"` 1px）引到顶部标注区。
- 下标 `SiO<tspan baseline-shift="sub" font-size="70%">2</tspan>`；上标 `n<tspan baseline-shift="super" font-size="70%">+</tspan>`。
- 说明文字中文；化学式/缩写（SiO₂、W、USG、P-Well）保留原形。
- 字号：区内 15~18，引线 14~16，步骤标题 18~22 粗体 `#1A237E`、格式 `1) PECVD 氮化硅`；图注一行小字 `#666`。

## 配色（教材约定）

- 衬底/阱：P-Wafer `#F5B17B`｜P-Epi `#FFF59D`｜P-Well `#FAD7A0`｜N-Well `#A9DFBF`
- 扩散/栅：n⁺ `#58D68D`｜p⁺ `#F5995B`｜多晶硅栅 `#EC7063`
- 介质：SiO₂/USG/BPSG/PSG/FSG `#D5D8DC`（同族同色靠标注区分）｜STI `#E5E8E8`｜SiN `#AF7AC5`｜光刻胶 `#BB8FCE`
- 金属：W `#48C9E0`｜Al·Cu `#85C1E9`｜Cu `#F1948A`｜Ti/TiN·Ta/TaN `#4A6FA5`｜硅化物 `#F5B7B1`
- 表外新材料：选区分度高的中间饱和度色，说明一次，同对话保持一致。

## 正确性清单（画前核对，画错比画丑严重）

1. 热氧化界面下凹（约一半厚度陷入硅内）；淀积层（CVD/PVD）界面不动。
2. **LOCOS 鸟嘴**：场氧边缘横向钻入 SiN 掩膜下方的氧化物舌头，**与场氧同材质同填充、连成一体**，绝不是场氧中间的洞；必画 SiN + 衬垫氧化层，SiN 边缘被顶起；有效有源区 < SiN 窗口。
3. STI：CMP 后与表面齐平，浅槽，无鸟嘴（这正是它取代 LOCOS 的原因）。
4. 接触孔上宽下窄；W 塞（CVD）填实；Void 只在讲 PVD 填充缺陷时画。
5. 栅与硅之间隔栅氧细线（2~3px）；源漏浅、在表面；大马士革 Cu 必被 Ta/TaN 阻挡层包覆；硅化物只在孔底硅表面/栅上。
6. KOH(100)：侧壁 54.7°、平底，不画 45° 或垂直。DRIE：近垂直侧壁、高深宽比。两者绝不混用。
7. 悬空结构下方必画空腔，锚点落地；键合界面一条加粗线。
8. 各向同性刻蚀有横向钻蚀；各向异性刻蚀垂直无钻蚀。CVD 保形覆盖，PVD 不保形。

## 布局防翻车

- SVG 外层 `style="max-width:100%;height:auto"`；单图 viewBox 宽 ≤1000，序列整体 ≤1200，任何元素不许被裁切。
- 多步序列：2 列网格 `align-items:start`，2 张一行 / 3~4 张 2×2 / 更多每行 2~3 张；**禁 3 张以上单行横排、禁长箭头串联**；流向靠编号标题表达。子图统一 viewBox 高度，内容占 ≥80%。
- 图内只允许：图形、标注、引线、标题、底部一行图注。解释性文字（结构要点、原理、列表）一律放图外对话正文，**禁压图浮层**。

## 输出前自检（不过不发）

1. **步骤完整**：编号连续无缺步；每步编号标题贴图上方、caption 贴图下方，无丢步、丢标题、丢 caption。
2. 无异常空白带；网格行距均匀（≤24px）。
3. 每种材料、每个结构都有文字（铁律 2 复核）。
4. 无裁切：最右、最下元素都在 viewBox 内。
5. 无压图：无文字块/卡片盖住图形。
6. 正确性清单中涉及本条目的款项全部通过。
7. 长序列（>6 步）分多次连续输出、每次给完整若干步；若被截断，主动补发剩余步骤并说明。

## 按需附录

- 附录 A：CMOS 模板与片段库——**已内联在下方**。
- 附录 B：MEMS 图元库——画 MEMS 前必须先读取：https://raw.githubusercontent.com/jinliang094-ctrl/device-fab-diagram/main/mems.md
- 附录 C：工艺流程框图规范——画框图前必须先读取：https://raw.githubusercontent.com/jinliang094-ctrl/device-fab-diagram/main/flowchart.md

---

# 附录 A：CMOS 剖面模板与片段库

改造优先：复制模板 SVG，按需增删层、改标注。坐标可整体平移缩放，不要重排相对结构。

## 完整 CMOS 剖面模板（双阱 + STI + 多晶硅栅 + W 塞 + M1）

```html
<div style="background:#fff;padding:10px;font-family:Arial,'PingFang SC','Microsoft YaHei',sans-serif">
<svg viewBox="0 0 900 500" style="max-width:100%;height:auto;display:block" xmlns="http://www.w3.org/2000/svg">
<rect x="60" y="380" width="780" height="60" fill="#F5B17B" stroke="#2C3E50" stroke-width="1.2"/>
<text x="420" y="416" font-size="17" fill="#4E342E">P-Wafer</text>
<rect x="60" y="340" width="780" height="40" fill="#FFF59D" stroke="#2C3E50" stroke-width="1.2"/>
<text x="430" y="366" font-size="16" fill="#5D4037">P-Epi</text>
<path d="M110,240 L400,240 L400,300 Q400,340 350,340 L160,340 Q110,340 110,300 Z" fill="#FAD7A0" stroke="#2C3E50" stroke-width="1.2"/>
<path d="M510,240 L800,240 L800,300 Q800,340 750,340 L560,340 Q510,340 510,300 Z" fill="#A9DFBF" stroke="#2C3E50" stroke-width="1.2"/>
<text x="200" y="315" font-size="17" fill="#5D4037">P-Well</text>
<text x="620" y="315" font-size="17" fill="#1B5E20">N-Well</text>
<g fill="#E5E8E8" stroke="#2C3E50" stroke-width="1.2">
<polygon points="90,240 170,240 158,282 102,282"/>
<polygon points="410,240 500,240 488,282 422,282"/>
<polygon points="760,240 840,240 828,282 772,282"/>
</g>
<g stroke="#2C3E50" stroke-width="1.2">
<polygon points="200,240 250,240 246,264 204,264" fill="#58D68D"/>
<polygon points="330,240 380,240 376,264 334,264" fill="#58D68D"/>
<polygon points="540,240 590,240 586,264 544,264" fill="#F5995B"/>
<polygon points="670,240 720,240 716,264 674,264" fill="#F5995B"/>
</g>
<g font-size="14">
<text x="205" y="258" fill="#1B5E20">n<tspan baseline-shift="super" font-size="70%">+</tspan></text>
<text x="335" y="258" fill="#1B5E20">n<tspan baseline-shift="super" font-size="70%">+</tspan></text>
<text x="545" y="258" fill="#7A3B00">p<tspan baseline-shift="super" font-size="70%">+</tspan></text>
<text x="675" y="258" fill="#7A3B00">p<tspan baseline-shift="super" font-size="70%">+</tspan></text>
</g>
<rect x="60" y="126" width="780" height="114" fill="#D5D8DC" stroke="#2C3E50" stroke-width="1.2"/>
<text x="88" y="190" font-size="16" fill="#333">SiO<tspan baseline-shift="sub" font-size="70%">2</tspan>（BPSG）</text>
<rect x="262" y="216" width="56" height="24" fill="#EC7063" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="602" y="216" width="56" height="24" fill="#EC7063" stroke="#2C3E50" stroke-width="1.2"/>
<g fill="#48C9E0" stroke="#2C3E50" stroke-width="1.2">
<polygon points="208,126 262,126 250,241 220,241"/>
<polygon points="343,126 397,126 385,241 355,241"/>
<polygon points="558,126 612,126 600,241 570,241"/>
<polygon points="703,126 757,126 745,241 715,241"/>
</g>
<rect x="180" y="96" width="230" height="30" fill="#85C1E9" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="530" y="96" width="230" height="30" fill="#85C1E9" stroke="#2C3E50" stroke-width="1.2"/>
<g font-size="15" fill="#222">
<text x="150" y="30">W</text>
<line x1="160" y1="36" x2="228" y2="120" stroke="#555" stroke-width="1"/>
<text x="420" y="30">M1，Al·Cu</text>
<line x1="470" y1="36" x2="470" y2="92" stroke="#555" stroke-width="1"/>
<text x="660" y="30">STI</text>
<line x1="672" y1="36" x2="455" y2="235" stroke="#555" stroke-width="1"/>
<text x="240" y="66">多晶硅栅</text>
<line x1="280" y1="72" x2="290" y2="212" stroke="#555" stroke-width="1"/>
</g>
</svg>
</div>
```

**最低交付清单**（弱模型照着补齐）：衬底、阱、STI、源漏扩散区、栅、ILD、塞、金属各就各位；每种材料有字；顶部引线标注区至少 3 条引线；底部一行图注。

## 多步剖面序列布局

```html
<div class="grid" style="display:grid;grid-template-columns:1fr 1fr;gap:24px;padding:16px;align-items:start;background:#fff;font-family:Arial,'PingFang SC','Microsoft YaHei',sans-serif">
<div><h3 style="margin:0 0 4px;color:#1A237E;font-size:19px">1) 工序名</h3>
<svg viewBox="0 0 440 260" style="width:100%;height:auto;display:block">…</svg>
<div style="color:#666;font-size:13px">一行图注</div></div>
<div><h3 style="margin:0 0 4px;color:#1A237E;font-size:19px">2) 工序名</h3>…</div>
</div>
```

规则：所有子图**共用同一套坐标与层叠**，只是逐步加层，读者一眼看出"哪变了"；新增层用引线明确指出；每步说明只放图下一行 caption。

## 常见变体

- **Al 直填接触孔**（老工艺）：去 W 塞，Al·Cu 梯形延伸进孔；孔倾角大时画黑色 Void 示意填洞缺陷。
- **Cu 大马士革**：Cu 区 `#F1948A` 嵌介质槽内，槽壁一圈 Ta/TaN 细边（`#4A6FA5` 2~3px）；双层 = 上下两层介质各开槽，Cu 连成一体。
- **硅化物接触**：扩散区与塞之间加 3~4px `#F5B7B1` 薄层，标注 TiSi₂/CoSi₂。
- **栅氧**：栅下 2~3px 深色细线。