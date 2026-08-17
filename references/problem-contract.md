# 数字合同

模型最常见的翻车：题面写 24 和 16，讲解写 4 cm/h，答案再口算一个 4 小时，三处对不上。

**只允许一处放原始数据。下游全部算出来。旁白禁止数字字面量。**

## 最低结构

```js
const PROBLEM = {
  id: '…',                 // 英文短 id
  chapterLabel: 'Grades 4–8',
  titleCN: '蜡烛燃烧',
  titleEN: 'two candles, one time',
  family: 'uniform-rate',  // 或 'counting' / 'discrete' / 'bar'
  // ……本族字段
  statement: '……<span class="hl">{…}</span>……' // 只引用 PROBLEM 自己的字段
};

function solveFor(problem /* , 可选的闯关覆盖 */) { /* 纯函数，无 DOM */ }

const SOLVE = solveFor(PROBLEM);
```

`bootProblem()` 在启动时用 `PROBLEM` 填标题、题面、答案卡标签、控件名字。克隆新数字只改 `PROBLEM`。

## 旁白怎么写

```js
// 对
typewriter(`甲每小时烧掉 <span class="em">${S.rateA}</span> cm。`);

// 错 —— 数字是你脑子里算的
typewriter(`甲每小时烧掉 <span class="em">4</span> cm。`);
```

交稿前：在教学函数和 `typewriter(` 字符串里搜 `/\d/`。出现的数字必须能指回 `P.` / `S.`。允许留下的只有无意义的 0/1（「先走 1 小时」里的 1 如果是单位步长，也应来自 `P.timeStep`）。

## 插值

```js
function interp(tpl, ctx) {
  return tpl.replace(/\{([\w.]+)\}/g, (_, path) =>
    path.split('.').reduce((o, k) => (o != null ? o[k] : ''), ctx)
  );
}
```

`{actors.0.height}` 这种路径要能工作；或者不用路径，启动时自己拼题面。

## 两族字段

### 计数族 `family: 'counting'`

```js
kindA: { id, name, legs, color },
kindB: { id, name, legs, color },
totalHeads, totalFeet,
```

`solveFor` 必须给出：`allALegs, legGap, legDiff, countA, countB, verifyA, verifyB, verifyTot`。
闯关：先抽整数 `countA, countB`，再算 heads/feet。

### 匀速族 `family: 'uniform-rate'`

```js
actors: [
  { id, name, height, burnHours, color }, // 蜡烛：height + burnHours
  // 行程可改成 { id, name, start, rate, direction }
],
question: 'equal-height' | 'remaining-at-t' | 'remaining-ratio',
```

`solveFor` 必须给出：每个 actor 的 `rate`，目标时刻 `tStar`，那时各 actor 的剩余/位置，以及 `valid`。
闯关：先抽整数 `tStar` 和整数速率，再反推 `height = remain + rate * tStar`。

单位在 `PROBLEM` 里写清（cm、小时、里、分钟）。展示时刻优先「4 小时」这种干净数；不得已再用「1 小时 20 分」，换算也走 `SOLVE`。

## 合法性

`SOLVE.valid` 为假就不要开讲。默认题必须 valid。出题滑杆如果滑到无解，预览里直说「这组数没有正的交点」，不要假装有答案。
