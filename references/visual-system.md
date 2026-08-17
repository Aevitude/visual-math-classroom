# 视觉系统

教室，不是 SaaS，不是儿童贴纸书。

## 默认纸墨（计数族、白天课堂）

金样 `examples/chickens-rabbits.html` 的 tokens 可直接抄：

```css
--bg: #FBF8F3;
--surface: #FFFFFF;
--surface-soft: #F5F0E8;
--ink: #2B2825;
--ink-soft: #6B6158;
--ink-faded: #A69E93;
--line: #E8E3DA;
--accent: #B85C38;      /* 唯一强调 */
--success: #4A7C59;
```

字体：中文 `Noto Serif SC` + `Noto Sans SC`；英文数字 `Fraunces`。最多这三族。不要 Inter，不要系统默认一锅端。

## 允许换温度，不许换结构

匀速 / 蜡烛可以改成黄昏：

```css
--bg: #1C1612;
--surface: #2A221C;
--ink: #F7F3EB;
--accent: #E08A3C;      /* 焰色，仍是唯一强调 */
```

换了底，字色必须一起换。仍然：一种强调、纸或夜两种气质之一、禁止第三套皮肤。

## 禁止

- 紫、靛、品红当品牌色
- 大面积彩虹渐变、玻璃拟态堆叠、仪表盘网格
- emoji 当图标或动物/蜡烛
- 照片、图库、AI 写实图当主体
- 一张卡片里五种颜色
- 所有圆角都一样大（外层要比内层大一圈）

## 布局

桌面：左舞台右面板，`grid-template-columns: 1fr clamp(360px, 28%, 640px)`。
`html, body` 桌面 `overflow: hidden`，`.app` 撑满一屏。面板内部才允许细滚动。
`max-width: 960px` 改单栏，恢复 `overflow: auto`。

数字用 `tabular-nums`，变化时轻微 pulse。命中目标加 `.target` 变成 success 色。

## 开场

黑底或深墨底。一点光 → 英文小标签 → 中文题名 → 一句来历或副题 → 「轻触任意处」。
可 8–10 秒后自动进。进场后底盘淡入。这是进教室，不是 Logo 动画。

## 运动

- 入场：短 overshoot（金样里的 `--ease-spring`）
- UI：180–240ms，`cubic-bezier(0.4, 0, 0.2, 1)`
- 证明动画可以慢（蜡烛一小时一小时矮下去），但每段要能被 `runToken` 打断
- `prefers-reduced-motion: reduce` 时关掉持续晃动，保留状态切换
