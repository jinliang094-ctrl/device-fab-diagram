# 附录 B：MEMS 剖面图元库（device-fab-diagram 按需附录，画 MEMS 前读本文件）

MEMS 剖面与 CMOS 的差异：有空腔、有悬空结构、有各向异性腐蚀斜面。绘制顺序同样"先底后顶"，空腔（白+虚线）最后画以压住镂空位置。配色与标注规范同主文件。

## 体硅加工：KOH V 型槽

(100) 硅片各向异性腐蚀，侧壁为 (111) 面，**与水平夹角 54.7°**（画约 55°），槽底平直。槽深 ≈ 0.7 ×（开口宽 − 底宽）/2，示意图不必精确，但斜壁必须一眼是 55°，不画 45° 或垂直。

```svg
<svg viewBox="0 0 500 300" style="max-width:100%;height:auto">
<rect x="60" y="100" width="120" height="12" fill="#D5D8DC" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="300" y="100" width="140" height="12" fill="#D5D8DC" stroke="#2C3E50" stroke-width="1.2"/>
<rect x="60" y="112" width="380" height="130" fill="#F5B17B" stroke="#2C3E50" stroke-width="1.2"/>
<polygon points="180,112 300,112 252,198 228,198" fill="#fff" stroke="#2C3E50" stroke-width="1.2" stroke-dasharray="6,3"/>
<text x="190" y="60" font-size="15" fill="#222">KOH 腐蚀窗口</text>
<line x1="240" y1="66" x2="245" y2="105" stroke="#555"/>
<text x="320" y="170" font-size="14" fill="#222">(111) 侧壁，54.7°</text>
<line x1="330" y1="176" x2="278" y2="166" stroke="#555"/>
</svg>
```

## 体硅加工：背腔薄膜

背面腐蚀穿大部分衬底，正面留薄膜（压力传感器、麦克风）：衬底画厚（占图高 50%+），背腔是底部开的大梯形空腔（同 55° 侧壁），顶部留 5~10px 薄膜层用醒目色（SiN 紫 `#AF7AC5`），标注"敏感膜"。空腔白底虚线描边。

## 表面硅加工：牺牲层与悬空梁

释放前：衬底 / 牺牲层（SiO₂ 灰）/ 结构层（poly-Si 红）三明治。释放后：结构层中段悬空、两端锚点落地。

```svg
<g stroke="#2C3E50" stroke-width="1.2">
<rect x="60" y="200" width="380" height="50" fill="#F5B17B"/>
<rect x="60" y="188" width="90" height="12" fill="#EC7063"/>
<rect x="350" y="188" width="90" height="12" fill="#EC7063"/>
<rect x="60" y="170" width="380" height="18" fill="#EC7063"/>
<rect x="150" y="188" width="200" height="12" fill="#fff" stroke-dasharray="6,3"/>
</g>
<text x="200" y="150" font-size="15">释放后的悬臂梁（poly-Si）</text>
```

规则：悬空部分下方必须画出空腔（白+虚线）——这是"悬空"的唯一视觉证据；锚点 = 结构层穿过牺牲层落地的墩子，与梁同色；释放前后对比常画成两步序列，复用主文件附录 A 的序列布局。

## DRIE 深槽

高深宽比、侧壁近垂直（88°~90°）：槽画成窄高矩形空腔（白+虚线），侧壁垂直——与 KOH 的 55° 斜壁形成区分，读者靠侧壁角度认工艺。槽底可带轻微圆弧；"细化"阶段可加扇形波纹（scallop）。

## 晶圆键合

两片晶圆对扣，界面处一条**加粗线**（`stroke-width="2.5"`），标注"键合界面（阳极/直接键合）"。中间有空腔时在下片先刻好腔（白+虚线），上片盖合。

## MEMS 标注用词

掩膜层、腐蚀窗口、敏感膜、牺牲层、结构层、锚点、空腔、键合界面、深槽。材料名（SiO₂、SiN、poly-Si）保留符号。