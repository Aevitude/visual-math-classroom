---
name: visual-math-classroom
description: 复制 template.html，只改开头的 CARD，交回一个完整 HTML。不要只交 JSON。
---

# 交付物是一个 HTML 文件

不要只交 JSON。不要写说明长文。不要打开 examples。

1. 打开并复制整份  
   https://github.com/Aevitude/visual-math-classroom/blob/main/template.html
2. **只改**文件里 `const CARD = { ... }` 这一段，改成客户的题。
3. 上面的 CSS、下面的引擎一行都不要动。
4. 把改好的**完整 HTML**交回来。

## CARD 怎么填

先选 `stage`：

- `axis` 一条线：追及、相遇、行程。角色用 `{ name, start, rate }`。相对开：一边 start=0 rate=正，一边 start=全程 rate=负。
- `bars` 两条杠：蜡烛、注水。角色用 `{ name, height, hours }`。
- `grid` 一堆点：鸡兔、头脚。角色用 `{ name, legs }`，外加 `heads` `feet`。

旁白里的数字必须用占位符：`{a.name}` `{a.rate}` `{b.start}` `{tEqual}` `{meet}` `{distance}` `{rateSum}` `{countA}` `{heads}` `{feet}`

`t` 用数字或 `"equal"`。至少两种讲法。阿临短句，不要「首先我们来学习」。

客户没给数字，就用模板里那道「两车相遇」。
