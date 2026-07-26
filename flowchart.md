# 附录 C：工艺流程框图规范（device-fab-diagram 按需附录，画框图前读本文件）

工序用框、流向用箭头，直写 SVG。风格与剖面图一致：白底、`#2C3E50` 描边、中文标注、SVG 内不写注释。

## 图元约定

- **工序框**：圆角矩形 `rx="8"`，白底、`#2C3E50` 描边 1.5px。
- **起止框**：stadium（圆角半径=高度一半），浅灰底 `#E5E8E8`，用于开始（来料片）/ 结束（成品）。
- **判断/分支**：菱形，浅黄底 `#FFF59D`，用于检验、返工分支；"是/否""合格/返工"字样写箭头旁。
- **模块分组**：大圆角虚线框罩住一组工序，底色 8% 透明灰，用于 FEOL/BEOL/各模块。
- **箭头**：`stroke="#2C3E50"` 1.5px + 实心三角箭头头。

## 布局规则

- 默认从左到右；超过 6 个工序换行蛇形排列（第一行左→右，第二行右→左，行间转折箭头连接）。
- 框高统一 44px，框宽随文字（内边距 ≥12px），框间距 ≥40px。
- 框内文字 14~16px，一行放不下两行；关键参数（温度、剂量、厚度）可写第二行小字。

## 骨架模板

```svg
<svg viewBox="0 0 980 220" style="max-width:100%;height:auto" font-family="Arial,'PingFang SC','Microsoft YaHei',sans-serif">
<defs><marker id="arr" markerWidth="10" markerHeight="8" refX="9" refY="4" orient="auto">
<polygon points="0,0 10,4 0,8" fill="#2C3E50"/></marker></defs>
<rect x="20" y="80" width="110" height="44" rx="22" fill="#E5E8E8" stroke="#2C3E50" stroke-width="1.5"/>
<text x="75" y="107" text-anchor="middle" font-size="15">来料硅片</text>
<g fill="#fff" stroke="#2C3E50" stroke-width="1.5">
<rect x="180" y="80" width="120" height="44" rx="8"/>
<rect x="350" y="80" width="120" height="44" rx="8"/>
<rect x="520" y="80" width="120" height="44" rx="8"/>
</g>
<g font-size="15" text-anchor="middle">
<text x="240" y="107">氧化</text>
<text x="410" y="107">涂胶 / 光刻</text>
<text x="580" y="107">干法刻蚀</text>
</g>
<g stroke="#2C3E50" stroke-width="1.5" fill="none">
<line x1="130" y1="102" x2="172" y2="102" marker-end="url(#arr)"/>
<line x1="300" y1="102" x2="342" y2="102" marker-end="url(#arr)"/>
<line x1="470" y1="102" x2="512" y2="102" marker-end="url(#arr)"/>
<line x1="640" y1="102" x2="747" y2="102" marker-end="url(#arr)"/>
</g>
<polygon points="830,78 905,102 830,126 755,102" fill="#FFF59D" stroke="#2C3E50" stroke-width="1.5"/>
<text x="830" y="107" text-anchor="middle" font-size="14">检验</text>
<path d="M830,126 L830,170 L410,170 L410,130" fill="none" stroke="#2C3E50" stroke-width="1.5" stroke-dasharray="6,4" marker-end="url(#arr)"/>
<text x="600" y="163" font-size="13" fill="#666">不合格 → 去胶返工</text>
</svg>
```

## 与剖面序列的配合

用户同时要"流程框图 + 每步剖面"时：先出完整框图，再按框图顺序出剖面序列；框图工序名与剖面步骤标题**用词保持一致**（如框图写"PECVD USG"，剖面标题就写"2) PECVD USG"）。