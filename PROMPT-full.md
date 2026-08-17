# 完整提示词（豆包读不了 GitHub 时用这一份）

把下面从「—— 开始 ——」到「—— 结束 ——」整段贴给豆包。
仓库能打开时，仍建议它去读金样：https://github.com/Aevitude/visual-math-classroom

—— 开始 ——

你是「可视化学堂」的课件作者。做一节「蜡烛燃烧」互动讲题。输出一个完整的单文件 HTML。

仓库（能打开就读）：https://github.com/Aevitude/visual-math-classroom
先读 SKILL.md，再读 references/genres/uniform-rate.md 和 examples/chickens-rabbits.html。

# 默认题（用户没给数字就用这个）

甲蜡烛高 24 cm，6 小时燃尽；乙蜡烛高 16 cm，8 小时燃尽。同时点燃，多久后一样高？

先在代码里写死数据源，再算，禁止在旁白里手写数字：

```js
const PROBLEM = {
  id: 'candle-burn',
  chapterLabel: 'Grades 4–8',
  titleCN: '蜡烛燃烧',
  titleEN: 'two candles, one time',
  family: 'uniform-rate',
  actors: [
    { id: 'jia', name: '甲', height: 24, burnHours: 6, color: '#E8C36A' },
    { id: 'yi',  name: '乙', height: 16, burnHours: 8, color: '#D4B48A' },
  ],
  question: 'equal-height',
  statement: '甲蜡烛高 <span class="hl">{actors.0.height}</span> cm，<span class="hl">{actors.0.burnHours}</span> 小时燃尽；乙蜡烛高 <span class="hl">{actors.1.height}</span> cm，<span class="hl">{actors.1.burnHours}</span> 小时燃尽。同时点燃，多久后一样高？'
};

function solveFor(P) {
  const A = P.actors[0], B = P.actors[1];
  const rateA = A.height / A.burnHours;
  const rateB = B.height / B.burnHours;
  const tEqual = (A.height - B.height) / (rateA - rateB);
  const remain = A.height - rateA * tEqual;
  return { rateA, rateB, tEqual, remain, rateDiff: rateA - rateB, valid: tEqual > 0 && remain >= 0 };
}
const SOLVE = solveFor(PROBLEM);
```

旁白、按钮、答案卡里的数字全部写成 `${S.tEqual}` / `${P.actors[0].height}` 这种插值。全文搜索旁白里的阿拉伯数字，对不上 SOLVE 就改。

# 产品形状（不许改布局）

```
[ 开场：黑底一点才进教室，标题「蜡烛燃烧」 ]
[ 章级标签 · 中文题名 · 英文副题 ]
[ 舞台：桌子 + 两支蜡烛 ]    [ 题面 ]
  当前时刻、两支剩余高度      [ 方法页签，仅教学 ]
  底部时间轴可拖              [ 探索：拖时间 / 点燃 ]
                             [ 阿临 + 主按钮 ]
                             [ 答案卡，解出才出现 ]
                             [ 四模式：探索 / 跟我学 / 闯关 / 出题 ]
```

# 硬禁令

- 禁止画鸡、兔、围栏、加减只数。主控件是时间轴。
- 禁止 emoji（蜡烛图标也不行）、禁止紫渐变、禁止仪表盘、禁止照片。蜡烛用手绘 SVG。
- 禁止「首先我们来看这道题」。老师叫阿临，打字机说话，按钮是悬念。
- 四模式齐。跟我学至少两种方法，最后同一答案。
- 每一步先改舞台再说话。
- 切换模式必须让旧的异步教学立刻停掉（`runToken++`）。
- 桌面一屏能看完（投影），不要长页滚动。
- 闯关必须先抽整数时刻再反推蜡烛数据，保证能解。
- 单文件，无 React / 无 npm。

# 舞台（匀速族 · 蜡烛）

- 黄昏桌面，暖光。两支蜡烛并排，蜡体高度 = 剩余长度，从底座往上长。
- 点燃时火苗抖动；熄灭时一缕烟。蜡油可有一点滴落，不要喧宾夺主。
- 探索：拖时间轴，两支按各自速率变矮。时刻与剩余高度用大号数字，对上目标时变绿。
- 闲时火苗自己晃，不要让蜡烛乱跳。

# 跟我学（至少两法，建议三法）

方法一 · 单位时间
1. 两支以原高出现，未点燃。
2. 算出每小时烧掉多少（height/hours），旁白插 SOLVE.rateA / rateB。
3. 时间一小时一小时走，每走一格两支按速率变矮，直到一样高。

方法二 · 剩余量方程
甲剩余 = hA − rateA·t，乙剩余 = hB − rateB·t，令相等，解 t。
可用一条「剩余高度随时间」的简图，两线相交的点就是答案。不要上复杂坐标系库。

方法三 · 速度差
一开始甲比乙高 (hA−hB)。甲每小时多烧掉 (rateA−rateB)。追上要用 t = 高差 / 速差。
画面上两支并排，高差用一根浅线标出来，随时间缩短。

每一步按钮文案预告下一句，例如「先让时间走一小时」「那高出来的那段谁消得更快」。

# 其他三模式

- 探索：学生自己拖时间，凑到两支一样高。凑对就庆祝、出示答案卡。
- 闯关：随机生成另一道「同时点燃，何时一样高」，整数解。先抽 t、rate、remain，再反推 height = remain + rate·t，burnHours = height/rate。
- 出题：滑杆改两支的高和燃尽时间，预览题面，可复制分享链接 `?h1=&t1=&h2=&t2=`。

# 视觉

纸色背景 `#FBF8F3`，墨色字 `#2B2825`，强调色用焰色 `#B85C38` 或蜡黄 `#E8C36A`，只取一种当主强调。
字体：中文宋/黑体 + 一个英文衬线（Fraunces 或类似）做数字和英文副题。
不要 Inter 紫、不要彩虹标签、不要卡片堆砌的 SaaS 后台。

# 阿临

短句，会停顿。可以说「先别急着列式，看它们怎么矮下去」。
不要幼教撒娇，不要「同学们今天我们来学习」。

# 交稿前自检（回复里列出勾选）

- [ ] 单文件 HTML
- [ ] 开场仪式
- [ ] 四模式 + runToken
- [ ] 旁白无手写数字
- [ ] 教学 ≥ 2 法，答案一致
- [ ] 每步舞台有变化
- [ ] 主控件是时间轴
- [ ] 无 emoji / 无紫 / 无教育后台皮肤
- [ ] 蜡烛是手绘 SVG，火苗会动
- [ ] 桌面一屏
- [ ] 闯关保证能解
- [ ] 出题可预览
- [ ] 答案卡只在解出后出现

—— 结束 ——
