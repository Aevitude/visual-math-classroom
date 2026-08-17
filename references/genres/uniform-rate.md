# 题型族 · 匀速（蜡烛 / 注水 / 行程 / 工程）

东西按固定速率变少或变多。问：何时一样、某时刻还剩多少、还要多久。

**蜡烛燃烧走这里，不走鸡兔。** 主变量是时间。主控件是时间轴。

同族换皮：

| 题 | actor 的「量」 | 速率 | 命中画面 |
|---|---|---|---|
| 蜡烛 | 剩余高度 | cm / 小时 | 两焰齐平 |
| 注水排水 | 水面高度 | 升 / 分 | 两液面齐 |
| 行程 | 位置 | 里 / 时 | 两人相遇或追及 |
| 工程 | 剩余工作 | 份 / 天 | 做完 = 0 |

## 默认题（用户只说「蜡烛燃烧」就用这个）

> 甲蜡烛高 24 cm，6 小时燃尽；乙蜡烛高 16 cm，8 小时燃尽。同时点燃，多久后一样高？

```js
actors: [
  { id:'jia', name:'甲', height:24, burnHours:6, color:'#E8C36A' },
  { id:'yi',  name:'乙', height:16, burnHours:8, color:'#D4B48A' },
],
question: 'equal-height',
```

```js
function solveFor(P) {
  const A = P.actors[0], B = P.actors[1];
  const rateA = A.height / A.burnHours; // 4
  const rateB = B.height / B.burnHours; // 2
  const tEqual = (A.height - B.height) / (rateA - rateB); // 4
  const remain = A.height - rateA * tEqual;               // 8
  return {
    rateA, rateB, tEqual, remain,
    heightGap0: A.height - B.height,     // 8
    rateDiff: rateA - rateB,             // 2
    maxHours: Math.max(A.burnHours, B.burnHours),
    valid: Number.isFinite(tEqual) && tEqual > 0 && remain >= 0
           && tEqual <= A.burnHours && tEqual <= B.burnHours
  };
}
```

旁白里出现的 24、16、6、8、4、2、8 cm、4 小时，必须全部来自 P/S。

当前时刻 `state.t`（探索由学生拖，教学由剧本赋）。剩余：

```
remainA(t) = max(0, A.height - rateA * t)
remainB(t) = max(0, B.height - rateB * t)
```

蜡体高度按这个画。燃尽后火灭。

## 主控件

一条时间轴，范围 `[0, SOLVE.maxHours]`。可拖、可点刻度。
教学时禁用拖动，由步骤去 `setT(t)`，蜡体用 400–700ms 过渡。
计量：当前时刻、甲剩余、乙剩余。两剩余相等且 t>0 → 命中。

不要加减「蜡烛只数」。那是计数族的控件。

## 舞台

见 `references/stage-metaphor.md` 蜡烛专项。摘要：

- 桌子，两支蜡烛，底座对齐，蜡体高度 = 剩余，火苗在顶上晃
- 底部时间轴
- 等高时一根水平虚线
- 黄昏暖光可以，院子草地不要

## 三法（跟我学）

### 1. 单位时间

1. 两支以原高出现，未点燃。报原高、燃尽时间（插 P）。
2. 点出速率：`每小时烧掉 height/burnHours`（插 S.rateA / S.rateB）。火苗点着。
3. 一小时一小时走。每步两支按速率变矮，阿临报这一小时后的剩余。
4. 走到 `tEqual`，齐平。验证 `remain`。

### 2. 剩余量方程

剩余甲 = hA − rateA·t，剩余乙 = hB − rateB·t。令相等。
可在舞台上叠一张很淡的「剩余–时间」简图：两条下降直线，交点标 `(tEqual, remain)`。不要上 D3。
交点出现后再让蜡烛跳到该时刻验证。

### 3. 速度差（建议做，对应鸡兔的抬腿法——巧思）

一开始甲比乙高 `heightGap0`。
甲每小时多消掉 `rateDiff`。
`tEqual = heightGap0 / rateDiff`。
画面：两支并排，高差用浅色括号或虚线标出，随时间缩短，缩到 0 即相等。

三法最后都必须把答案卡写成：时刻 = `S.tEqual`，那时各剩 `S.remain`。

## 闯关（答案先行）

```js
const tStar   = randInt(2, 6);
const rateA   = randInt(3, 6);          // 甲更快
const rateB   = randInt(1, rateA - 1);
const remain  = randInt(2, 10) * 2;
const hA = remain + rateA * tStar;
const hB = remain + rateB * tStar;
const burnA = hA / rateA;
const burnB = hB / rateB;
```

保证正剩余、甲先燃尽或同时、tStar 时两支都还在烧。
题面只给高和燃尽时间，不给速率。

## 出题

四条滑杆：甲高、甲小时、乙高、乙小时。预览题面 + 若 `valid` 写出「别告诉他们：t 小时后剩 r」。
无解（速率相同且原高不同、或交点在燃尽之后）要直说。
分享：`?h1=24&t1=6&h2=16&t2=8`。收到链接进闯关，舞台清零。

## 其他问法（有人点名再换，不要默认做）

- `remaining-at-t`：点了 t0 小时，各剩多少？探索控件仍是时间轴，目标是拖到 t0。
- `remaining-ratio`：点到何时甲剩的是乙的一半？`solveFor` 改成解剩余比例方程。
- 两支一样长、燃尽时间不同：等高发生在 t=0，不要拿来当默认题。

## 禁做

- 禁止画成鸡兔围栏换两支蜡
- 禁止主控件是「蜡烛 + / −」
- 禁止用 emoji 蜡烛
- 禁止两支用同一速率（没戏）
- 禁止教学不拨时间、只在旁边列式
- 禁止闯关随手乱抽高和小时导致永远一样高不了
