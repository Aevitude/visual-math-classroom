# 题型族 · 计数（鸡兔同笼）

两种东西，每只「头」1 个，脚数不同。已知头总数、脚总数，求各几只。

金样：`examples/chickens-rabbits.html`。换螃蟹/蜻蜓/三轮车时，只改 `PROBLEM` 和 SVG，底盘不动。

## PROBLEM

```js
kindA: { id:'chicken', name:'鸡', legs:2, color:'#E8B547' },
kindB: { id:'rabbit',  name:'兔', legs:4, color:'#BEB5A6' },
totalHeads: 35,
totalFeet: 94,
```

默认金样数：23 鸡 + 12 兔。`solveFor` 必须给出：

```
allALegs = h * a          // 35*2 = 70
legGap   = f - allALegs   // 94-70 = 24
legDiff  = b - a          // 2
countB   = legGap/legDiff // 12
countA   = h - countB     // 23
```

旁白禁止出现 35/94/70/24/12/23 这些字面量。

## 主控件

鸡 +/- 、兔 +/-。点按 +1，长按加速。上限约 40。
计量：头、脚。双中且非零 → 命中。

## 舞台

围栏。两种动物 SVG，脚要能数。鸡闲时啄，兔闲时喘。假设法里鸡变兔要有 morph。抬腿法：鸡抬 2 脚离地，兔抬 2 脚后仰仍沾地——这就是证明，两套动画不能写成同一个 float。

## 三法

1. **假设法**：先铺满 kindA → 报 allALegs → 点出 legGap → 一只一只换成 kindB。
2. **方程组**：x+y=heads，a x + b y = feet。舞台里画两条线，交点是答案。再撤回动物验证。
3. **抬腿法**：按真实构成进笼（学生还不知道）→ 全体抬 kindA.legs 只脚 → 地上只剩 kindB 的多余脚 → 除以 legDiff。

## 闯关 / 出题

闯关：`countB ∈ [3,14]`，`countA ∈ [5,19]`，反推 heads/feet。
出题：两只滑杆，预览「几头几脚」。分享 `?a=&b=` 或只分享头脚。

## 禁做

- 不要用饼图或柱状图代替动物
- 不要第三种动物
- 不要在蜡烛课里复用本文件
