# 可视化学堂 Skill

让大模型（豆包 / Claude / 其他）做出和 `examples/chickens-rabbits.html` 同级的互动数学课件。

仓库：https://github.com/Aevitude/visual-math-classroom

质量不靠「做漂亮一点」，靠几条硬合同：数字只从 `PROBLEM` / `solveFor` 来、动画本身就是证明、四模式齐、一种题型一个舞台。

## 给豆包的用法

把仓库地址和 `PROMPT.md` 一起贴给豆包。若它读不了 GitHub，改贴 `PROMPT-full.md`。

验收题：让它做一节「蜡烛燃烧」。过关标准见 `SKILL.md` 文末清单。

## 目录

```text
SKILL.md                 ← 主手册，模型必读
PROMPT.md                ← 短提示词
PROMPT-full.md           ← 仓库读不了时用
examples/
  chickens-rabbits.html  ← 金样。质量地板。克隆底盘用。
references/
  philosophy.md
  problem-contract.md
  pedagogy-loop.md
  teacher-voice.md
  visual-system.md
  stage-metaphor.md
  anti-slop.md
  chassis-contract.md
  genres/
    chickens-rabbits.md  ← 计数族（鸡兔、头脚）
    uniform-rate.md      ← 匀速族（蜡烛、注水、行程、工程）
```

## 不要指望它做什么

- 不会自动变成 React / 小程序。默认就是一个 HTML。
- 现在稳住两族：计数、匀速。新题型按 `SKILL.md` 决策树发明隐喻，底盘不许拆。
- 金样是鸡兔同笼。蜡烛必须走匀速族，禁止把蜡烛画进鸡笼。
