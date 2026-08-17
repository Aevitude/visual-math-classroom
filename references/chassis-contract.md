# 底盘：从金样克隆时留什么、换什么

金样：`examples/chickens-rabbits.html`（约 86 KB，单文件）。
正确姿势是**复制金样再换心脏**，不是从空白 invent 一个教育网站。

## 留下（一行都不要「重构」掉）

- 整段 `:root` tokens 和纸墨 CSS（蜡烛可按 `visual-system.md` 改温度，结构不动）
- `.intro` 开场仪式
- `.app` / `.header` / `.main` / `.stage` / `.panel` 栅格
- 投屏：桌面 `overflow: hidden` + fit-to-viewport；手机恢复滚动
- 触控：去掉 tap 高亮、按钮 `touch-action: manipulation`、UI 禁选中、题面可复制
- `interp`、`typewriter`、`typewriterToken`、`runToken`、`sleep`
- `setMode` / 四按钮 / 方法页签的显示逻辑
- 阿临卡片、主按钮、答案卡、toast、声音开关、Web Audio 小函数
- `celebrate`（叶子或火星都可以，风格跟着隐喻）
- `bootProblem`：标题、题面、答案标签从 PROBLEM 灌进 DOM
- 分享链接读写（query 字段随族改）

## 换掉

| 金样里的 | 换成（蜡烛例） |
|---|---|
| `PROBLEM.kindA/kindB/totalHeads/totalFeet` | `family:'uniform-rate'` + `actors[]` |
| `solveFor(problem, heads, feet)` | `solveFor(problem)` 算 rate / tEqual / remain |
| `#chicken` `#rabbit` symbol | `#candle-jia` `#candle-yi` 或一支可染色的 symbol |
| `.pen` 院子、草、太阳斑、动物 grid | 桌子 + 两支锚定在底边的蜡烛 |
| 鸡+/- 兔+/- | 时间轴 |
| `renderPen` | `renderStage`：按当前 t 设蜡体高度 |
| `runAssumption` / `runEquation` / `runAncient` | 单位时间 / 剩余方程 / 速度差 |
| 假设法变种、抬腿离地 | 删。不要留一只鸡在蜡烛课里 |

## 工程陷阱（金样已经踩过，照抄）

1. **`runToken`**：`setMode` / `setMethod` 里 `++`。每个 `runXxx` 开头 `const myToken = runToken`，每次 `await` 后核对。
2. **列表不要用 live `children` 边删边走**。金样 `renderPen` 用静态数组，就是因为这个死循环过。
3. **打字机先写 HTML 再填文本节点**，不要 `innerHTML += char`，否则 span 结构会乱。
4. **长按步进**（计数族）要 `pointerup/leave/cancel` 清 timer，否则会自己加到 40。
5. 声音默认关；第一次手势里 `ensureAudio()`。

## 不要做的「优化」

- 不要拆成 React / Vue / 多文件
- 不要引入 Chart.js、Tailwind CDN、一堆 icon font
- 不要把老师改成右下角聊天气泡
- 不要加登录、积分、排行榜
- 不要把四模式收进汉堡菜单
