# 意识形态测试 SPA 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a single-page ideological test with ~140 questions, 5 axes, 100 ideologies, avant-garde visual style, matching algorithm, and Chart.js radar chart to GitHub Pages.

**Architecture:** Static SPA — single `index.html` with CDN-loaded Tailwind CSS and Chart.js, two JSON data files loaded via `fetch()`, all JS logic inlined. Zero build step.

**Tech Stack:** Vanilla HTML/JS, Tailwind CSS (CDN), Chart.js (CDN), Canvas API for dynamic background, CSS `@keyframes` for glitch effects.

---
## File Structure

```
/
├── index.html              ← Main app (HTML structure + Tailwind CDN + custom CSS + all JS logic in <script>)
├── questions.json          ← ~140 questions with 5-axis weights (-2..+2)
├── ideologies.json         ← 100 ideologies with 5-axis coords (0..100), categories, descriptions
└── .github/
    └── workflows/
        └── pages.yml       ← GitHub Actions: deploy static files to Pages on push to main
```

---

### Task 1: Create `questions.json` — 140+ questions with 5-axis weights

**Files:**
- Create: `questions.json`

- [ ] **Step 1: Create the JSON structure and first 100 questions from original document**

The JSON format:
```json
[
  {
    "id": 1,
    "text": "生产资料的最终所有权应当由全社会的劳动者共同掌握。",
    "weights": {
      "econ": 2,
      "diplomatic": 0,
      "state": 1,
      "society": 1,
      "tech": 0
    }
  }
]
```

Assign multi-axis weights for the original 100 questions as follows:

**Rules for multi-axis weight assignment:**
- Original document weight ±1 becomes magnitude ±1 or ±2 on the primary axis
- Multi-axis when the question logically spans dimensions
- Tech axis assigned when question involves: AI, automation, surveillance, internet, data privacy, genetic engineering, crypto, cyber

**Multi-axis examples from original 100:**

| # | Question Text | Primary Axis | Primary W | State | Tech | Society | Econ | Dipl |
|---|---|---|---|---|---|---|---|---|
| 1 | 生产资料的最终所有权应当由全社会的劳动者共同掌握。 | econ | +2 | +1 | 0 | 0 | 0 | 0 |
| 2 | 自由市场的价格机制在分配资源时... | econ | -2 | 0 | 0 | 0 | 0 | 0 |
| 5 | 应对自动化和AI...实施全民基本收入（UBI）是绝对必要的 | econ | +2 | 0 | +1 | 0 | 0 | 0 |
| 6 | 不受地理限制的跨国巨头公司在本质上损害了全球底层民众的利益 | econ | +1 | 0 | 0 | 0 | 0 | +1 |
| 7 | 医疗、教育等所有公共服务都应当被完全私有化 | econ | -2 | -1 | 0 | 0 | 0 | 0 |
| 12 | 供水、电力、公共交通等基础生命线行业必须由国家垄断运营 | econ | +2 | +1 | 0 | 0 | 0 | 0 |
| 20 | 知识产权法已经变异成了资本垄断技术的工具 | econ | +1 | 0 | +1 | 0 | 0 | 0 |
| 26 | 我的国家在本质上都优于世界上绝大多数国家 | dipl | 0 | +1 | 0 | +1 | 0 | +2 |
| 28 | 最终应当打破所有国界，建立一个统一的地球联邦政府 | dipl | 0 | -1 | 0 | -1 | 0 | -2 |
| 30 | 拥有一支拥有压倒性优势的强大武装部队... | diag | 0 | +1 | 0 | 0 | 0 | +2 |
| 33 | 边界线不过是掌权者为了划分统治范围而虚构的线条 | dipl | 0 | -1 | 0 | -1 | 0 | -2 |
| 39 | 移民必须彻底放弃原有习俗，完全同化... | dipl | 0 | +1 | 0 | +1 | 0 | +1 |
| 40 | 多元文化主义是国家的优势而非劣势 | dipl | 0 | -1 | 0 | -1 | 0 | -2 |
| 43 | 向外进行军事扩张在历史角度上是合理的 | dipl | +1 | +1 | 0 | 0 | 0 | +2 |
| 45 | 我国公民的生命权必然比地球另一端外国人的生命更具优先价值 | dipl | 0 | 0 | 0 | 0 | 0 | +2 |
| 48 | 维护血统和文化纯洁性对保持国家稳定至关重要 | dipl | 0 | +1 | 0 | +2 | 0 | +1 |
| 51 | 强有力且不受制约的领导人能更有效率地治理国家 | state | 0 | +2 | 0 | +1 | 0 | 0 |
| 52 | 两个成年人之间完全自愿的私人行为，国家无权干涉 | state | 0 | -2 | 0 | 0 | 0 | 0 |
| 53 | 政府对公民网络和通讯进行无差别大规模监听是必要的 | state | 0 | +2 | +2 | 0 | 0 | 0 |
| 54 | 所有毒品的个人吸食行为都应当被去罪化甚至合法化 | state | 0 | -2 | 0 | -1 | 0 | 0 |
| 55 | 极端政治言论威胁国家制度时，政府审查是正当的 | state | 0 | +2 | +1 | +1 | 0 | 0 |
| 56 | 允许公民合法持有枪支是抵抗暴政的最后防线 | state | 0 | -2 | 0 | 0 | 0 | 0 |
| 58 | 个人自由边界只能是不直接侵害他人的身体或财产 | state | 0 | -2 | 0 | 0 | 0 | 0 |
| 60 | 斯诺登这样的吹哨人应当被视为英雄而非叛徒 | state | 0 | -2 | +1 | 0 | 0 | 0 |
| 62 | 民众拥有使用武装暴力推翻压迫政府的权利 | state | -1 | -2 | 0 | 0 | 0 | 0 |
| 64 | 真正的无政府状态意味着人类获得真正的自由 | state | 0 | -2 | 0 | 0 | 0 | 0 |
| 65 | 国家机器存在本身就是对人类天然权利的强制侵犯 | state | 0 | -2 | 0 | 0 | 0 | 0 |
| 67 | 所有形式的国家出版物审查制度都应当遭到坚决抵制 | state | 0 | -2 | -1 | 0 | 0 | 0 |
| 70 | 赌博、卖淫、自残等受害者缺席的犯罪不应当被定义为犯罪 | state | 0 | -2 | 0 | 0 | 0 | 0 |
| 71 | 公共危机时个人隐私权必须被强制搁置 | state | 0 | +2 | +1 | 0 | 0 | 0 |
| 72 | 只有一党制或威权政府才能执行长远的国家战略 | state | 0 | +2 | 0 | +1 | 0 | 0 |
| 75 | 政府必须直接接管或强力控制互联网媒体 | state | 0 | +2 | +2 | +1 | 0 | 0 |
| 76 | 古老的宗教信仰和传统价值观应当作为社会道德的基石 | society | 0 | +1 | 0 | +2 | 0 | 0 |
| 77 | 同性恋伴侣应拥有平等婚姻权和收养权 | society | 0 | -1 | 0 | -2 | 0 | 0 |
| 78 | 除特殊情况外，堕胎应当被法律严格禁止 | society | 0 | +1 | 0 | +2 | 0 | 0 |
| 79 | 基因编辑和神经接口改变人类生物属性是对造物主的亵渎 | society | 0 | 0 | -2 | +2 | 0 | 0 |
| 83 | 即使导致失业和工业衰退，激进的环保政策也是绝对必要的 | society | +1 | 0 | -1 | -1 | 0 | 0 |
| 84 | AI加速发展最终将把人类从苦役中解放出来 | society | 0 | 0 | -2 | -1 | 0 | 0 |
| 85 | 女性更天然的角色是回归家庭、抚育后代 | society | 0 | +1 | 0 | +2 | 0 | 0 |
| 86 | 跨性别者寻求医疗手段改变身份应得到社会的绝对包容 | society | 0 | -1 | 0 | -2 | 0 | 0 |
| 87 | 科学技术要超越人类脆弱的生物学极限，掌控进化方向 | society | 0 | 0 | -2 | -2 | 0 | 0 |
| 88 | 现代艺术和流行文化是道德堕落的象征 | society | 0 | 0 | 0 | +2 | 0 | 0 |
| 89 | 安乐死必须是一项合法的基本人权 | society | 0 | -1 | 0 | -2 | 0 | 0 |
| 90 | 学校应该鼓励学生进行祈祷或向传统致敬 | society | 0 | +1 | 0 | +2 | 0 | 0 |
| 91 | 轻度毒品去罪化是社会向医疗康复观念的文明转变 | society | 0 | -1 | 0 | -2 | 0 | 0 |
| 92 | 没有宗教信仰，人类道德必然会迅速崩塌 | society | 0 | 0 | 0 | +2 | 0 | 0 |
| 93 | 社会应通过系统性补偿政策弥补历史上对少数群体的压迫 | society | +1 | 0 | 0 | -2 | 0 | 0 |
| 94 | 学术界和媒体被极端的政治正确所绑架 | society | 0 | 0 | 0 | +2 | 0 | 0 |
| 95 | 传统节日剥离神圣内核是对文化根基的破坏 | society | 0 | 0 | 0 | +2 | 0 | 0 |
| 96 | 追求生物学永生或意识上传是文明进化的正确方向 | society | 0 | 0 | -2 | -2 | 0 | 0 |
| 98 | 社会构建必须由理性、逻辑和实证科学主导 | society | 0 | 0 | -1 | -2 | 0 | 0 |
| 99 | 社会进步的本质就是不断打破前人的旧有规范 | society | 0 | 0 | 0 | -2 | 0 | 0 |
| 100 | 我希望社会倒退回去与几百年前保持一致 | society | 0 | +1 | 0 | +2 | 0 | 0 |

For remaining questions not listed above (clean single-axis), use the original document's ±1 weight on the primary axis, and 0 on all other axes. Some may be bumped to ±2 for emphasis.

- [ ] **Step 2: Add ~40-50 curated CNValues questions with rewritten 5-axis weights**

Select from CNValues, removing semantic duplicates with original 100. Rewrite their ±15 to multi-axis ±2.

Example conversions:

```json
{
  "id": 101,
  "text": "面对暴力示威者，政府应当止暴制乱恢复秩序。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 2, "society": 1, "tech": 0 }
},
{
  "id": 102,
  "text": "每个人在互联网上匿名的权利都应当得到保证。",
  "weights": { "econ": 0, "diplomatic": 0, "state": -2, "society": 0, "tech": -2 }
},
{
  "id": 103,
  "text": "挑战国家主权和社会稳定的言论，不属于言论自由的范畴。",
  "weights": { "econ": 0, "diplomatic": 1, "state": 2, "society": 1, "tech": 1 }
},
{
  "id": 104,
  "text": "在房屋征收拆迁过程中，政府有权拒绝过高的拆迁补偿要求。",
  "weights": { "econ": 1, "diplomatic": 0, "state": 1, "society": 0, "tech": 0 }
},
{
  "id": 105,
  "text": "个人主义思想的流行让社会陷入原子化的危机。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 0, "society": 2, "tech": 0 }
},
{
  "id": 106,
  "text": "废除以警察和军队，以私人安保公司和民兵组织来维持治安。",
  "weights": { "econ": -1, "diplomatic": 0, "state": -2, "society": -1, "tech": 0 }
},
{
  "id": 107,
  "text": "罢工是一种懒惰自私的行为，给他人造成了麻烦。",
  "weights": { "econ": -2, "diplomatic": 0, "state": 1, "society": 1, "tech": 0 }
},
{
  "id": 108,
  "text": "色情内容败坏社会风气，有必要进行打击。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 1, "society": 2, "tech": 1 }
},
{
  "id": 109,
  "text": "世界各地的人民都有自决权，自由决定他们的政治地位。",
  "weights": { "econ": 0, "diplomatic": -2, "state": -1, "society": -1, "tech": 0 }
},
{
  "id": 110,
  "text": "全球变暖是迫在眉睫的威胁。",
  "weights": { "econ": 0, "diplomatic": -1, "state": 0, "society": -1, "tech": -1 }
},
{
  "id": 111,
  "text": "为了经济增长，可以默许对劳动者权益的一些损害。",
  "weights": { "econ": -2, "diplomatic": 0, "state": 1, "society": 0, "tech": 0 }
},
{
  "id": 112,
  "text": "雇佣劳动是资本家对劳动者的剥削和奴役。",
  "weights": { "econ": 2, "diplomatic": 0, "state": 0, "society": 0, "tech": 0 }
},
{
  "id": 113,
  "text": "没有规范限制的市场经济是最好的。",
  "weights": { "econ": -2, "diplomatic": 0, "state": -1, "society": 0, "tech": 0 }
},
{
  "id": 114,
  "text": "女性所受的压迫是剥削形式中最深刻的，且是其他各种压迫的基础。",
  "weights": { "econ": 1, "diplomatic": 0, "state": -1, "society": -2, "tech": 0 }
},
{
  "id": 115,
  "text": "科技进步不应当过快地改变社会。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 0, "society": 1, "tech": 2 }
},
{
  "id": 116,
  "text": "婚姻制度应当被废除。",
  "weights": { "econ": 0, "diplomatic": 0, "state": -1, "society": -2, "tech": 0 }
},
{
  "id": 117,
  "text": "中国正走在正确的道路上，即中国特色社会主义道路。",
  "weights": { "econ": 1, "diplomatic": 1, "state": 1, "society": 1, "tech": 0 }
},
{
  "id": 118,
  "text": "为了社会的大变革，必须热烈地迎接社会的大崩溃。",
  "weights": { "econ": 0, "diplomatic": 0, "state": -1, "society": -2, "tech": -1 }
},
{
  "id": 119,
  "text": "大麻应当合法化。",
  "weights": { "econ": 0, "diplomatic": 0, "state": -2, "society": -2, "tech": 0 }
},
{
  "id": 120,
  "text": "没有祖国，你什么都不是。",
  "weights": { "econ": 0, "diplomatic": 2, "state": 1, "society": 1, "tech": 0 }
},
{
  "id": 121,
  "text": "「全世界无产者联合起来」已经过时了。",
  "weights": { "econ": -2, "diplomatic": 1, "state": 0, "society": 0, "tech": 0 }
},
{
  "id": 122,
  "text": "若能得到良好的维护，核能将是优良的能量来源。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 0, "society": 0, "tech": -1 }
},
{
  "id": 123,
  "text": "人类不应食用或奴役其他动物。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 0, "society": -1, "tech": 0 }
},
{
  "id": 124,
  "text": "传统医学比现代医学更值得信任。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 0, "society": 2, "tech": 2 }
},
{
  "id": 125,
  "text": "应当大规模应用转基因生物以提高农业产量。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 0, "society": -1, "tech": -2 }
},
{
  "id": 126,
  "text": "族群之间存在天生的智商差异。",
  "weights": { "econ": 0, "diplomatic": 1, "state": 0, "society": 2, "tech": 0 }
},
{
  "id": 127,
  "text": "毛泽东是近代以来中国最伟大的人。",
  "weights": { "econ": 2, "diplomatic": 2, "state": 2, "society": 2, "tech": 0 }
},
{
  "id": 128,
  "text": "在我国居住的外国人应当拥有与本国公民同等的权利。",
  "weights": { "econ": 0, "diplomatic": -2, "state": -1, "society": -1, "tech": 0 }
},
{
  "id": 129,
  "text": "中小学校应当开设系统的性教育（包括多元性别教育）必修课。",
  "weights": { "econ": 0, "diplomatic": 0, "state": -1, "society": -2, "tech": 0 }
},
{
  "id": 130,
  "text": "太空殖民是解决地球资源枯竭的好办法。",
  "weights": { "econ": -1, "diplomatic": 0, "state": 0, "society": -1, "tech": -2 }
},
{
  "id": 131,
  "text": "退休年龄应当调低。",
  "weights": { "econ": 1, "diplomatic": 0, "state": 0, "society": 0, "tech": 0 }
},
{
  "id": 132,
  "text": "应当对进口产品加征关税来保护国内产业。",
  "weights": { "econ": 1, "diplomatic": 2, "state": 0, "society": 0, "tech": 0 }
},
{
  "id": 133,
  "text": "多党制不可避免地导致社会撕裂和政策短视。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 2, "society": 1, "tech": 0 }
},
{
  "id": 134,
  "text": "在一切出现我国地图的场景中，必须使用官方声明的正确国家版图。",
  "weights": { "econ": 0, "diplomatic": 1, "state": 1, "society": 1, "tech": 0 }
},
{
  "id": 135,
  "text": "社交媒体上充斥着虚假信息和敌对宣传，政府必须直接控制互联网媒体。",
  "weights": { "econ": 0, "diplomatic": 1, "state": 2, "society": 1, "tech": 2 }
},
{
  "id": 136,
  "text": "作为普通公民，不必关心政治。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 0, "society": 0, "tech": 0 }
},
{
  "id": 137,
  "text": "因人类行为而造成的物种灭绝是不可接受的。",
  "weights": { "econ": 0, "diplomatic": -1, "state": 0, "society": -1, "tech": 0 }
},
{
  "id": 138,
  "text": "中国人的民族劣根性是真实存在的。",
  "weights": { "econ": 0, "diplomatic": -2, "state": 0, "society": 0, "tech": 0 }
},
{
  "id": 139,
  "text": "中国应当像欧洲那样分成一系列小国家。",
  "weights": { "econ": 0, "diplomatic": -2, "state": -2, "society": -1, "tech": 0 }
},
{
  "id": 140,
  "text": "处于关键时刻的国家需要政治强人。",
  "weights": { "econ": 0, "diplomatic": 1, "state": 2, "society": 1, "tech": 0 }
},
{
  "id": 141,
  "text": "应当确立最低工资标准以让劳动者有尊严地活着。",
  "weights": { "econ": 2, "diplomatic": 0, "state": 0, "society": 0, "tech": 0 }
},
{
  "id": 142,
  "text": "做一个好主妇、好母亲，是女人最大的本事。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 0, "society": 2, "tech": 0 }
},
{
  "id": 143,
  "text": "国家和地区的边界应当被废除。",
  "weights": { "econ": 0, "diplomatic": -2, "state": -1, "society": -1, "tech": 0 }
},
{
  "id": 144,
  "text": "企业应当关心社会责任，而不只是股东利润。",
  "weights": { "econ": 1, "diplomatic": 0, "state": 0, "society": 0, "tech": 0 }
},
{
  "id": 145,
  "text": "生命的意义来自超越自我，而不是追求舒适。",
  "weights": { "econ": 0, "diplomatic": 0, "state": 0, "society": 1, "tech": 0 }
}
```

- [ ] **Step 3: Save questions.json**

```bash
git add questions.json
```

---

### Task 2: Create `ideologies.json` — 100 ideologies with 5-axis coords

**Files:**
- Create: `ideologies.json`

- [ ] **Step 1: Create the JSON with all 100 ideologies from the document**

Format:
```json
[
  {
    "id": 1,
    "name": "斯大林主义",
    "nameEn": "Stalinism",
    "category": 1,
    "coords": {
      "econ": 100,
      "diplomatic": 70,
      "state": 0,
      "society": 60,
      "tech": 30
    },
    "description": "斯大林执政时期形成的苏联社会主义模式，强调高度集权的计划经济、一党专政和严厉的思想控制。"
  }
]
```

**Tech axis assignment logic:**
- High Authority (low state score) + Traditional (high society) → Tech Control (40-70)
- High Liberty (high state score) + Progressive (low society) → Tech Acceleration (0-30)
- Centrist socially → moderate tech (40-60)
- Specific ideologies with known tech stances get adjusted accordingly (e.g., Technocracy → tech=5, Crypto-Anarchism → tech=0)

**Full coordinate table with Tech axis (first 30 shown — all 100 follow same pattern):**

**Category 1: 马克思列宁主义与威权共产主义**
| ID | Name | E | D | S | So | T |
|----|------|---|---|---|----|---|
| 1 | 斯大林主义 | 100 | 70 | 0 | 60 | 50 |
| 2 | 马克思列宁主义 | 90 | 50 | 10 | 40 | 40 |
| 3 | 毛泽东思想 | 100 | 80 | 10 | 70 | 60 |
| 4 | 托洛茨基主义 | 95 | 10 | 30 | 20 | 20 |
| 5 | 左翼共产主义 | 90 | 20 | 40 | 10 | 20 |
| 6 | 欧洲共产主义 | 80 | 30 | 50 | 30 | 30 |
| 7 | 委员会共产主义 | 95 | 10 | 60 | 20 | 30 |
| 8 | 正统马克思主义 | 85 | 30 | 30 | 40 | 30 |
| 9 | 国有社会主义 | 80 | 60 | 20 | 50 | 50 |
| 10 | 兵营共产主义 | 100 | 50 | 0 | 80 | 60 |
| 11 | 波萨达斯主义 | 100 | 0 | 10 | 0 | 10 |
| 12 | 霍查主义 | 95 | 90 | 0 | 80 | 60 |
| 13 | 铁托主义 | 75 | 50 | 20 | 50 | 40 |
| 14 | 卡斯特罗主义 | 85 | 70 | 15 | 40 | 40 |
| 15 | 早期马克思主义 | 85 | 20 | 40 | 20 | 20 |

**Category 2: 社会民主主义与左翼自由意志主义**
| ID | Name | E | D | S | So | T |
|----|------|---|---|---|----|---|
| 16 | 民主社会主义 | 75 | 30 | 60 | 30 | 30 |
| 17 | 社会民主主义 | 60 | 40 | 65 | 40 | 40 |
| 18 | 无政府共产主义 | 100 | 0 | 100 | 10 | 10 |
| 19 | 无政府工团主义 | 90 | 10 | 90 | 20 | 20 |
| 20 | 互助主义 | 60 | 20 | 80 | 40 | 30 |
| 21 | 自由意志社会主义 | 80 | 20 | 85 | 20 | 20 |
| 22 | 社会生态学 | 80 | 10 | 80 | 10 | 15 |
| 23 | 基尔特社会主义 | 70 | 40 | 60 | 50 | 40 |
| 24 | 21世纪社会主义 | 80 | 50 | 40 | 40 | 50 |
| 25 | 农业社会主义 | 80 | 60 | 50 | 80 | 60 |
| 26 | 基督教社会主义 | 75 | 40 | 50 | 80 | 50 |
| 27 | 空想社会主义 | 80 | 30 | 60 | 40 | 20 |
| 28 | 工团主义 | 70 | 30 | 60 | 30 | 30 |
| 29 | 生态社会主义 | 85 | 10 | 70 | 10 | 15 |
| 30 | 自治主义 | 80 | 10 | 85 | 20 | 20 |

**Category 3: 自由主义与进步主义**
| ID | Name | E | D | S | So | T |
|----|------|---|---|---|----|---|
| 31 | 社会自由主义 | 55 | 30 | 70 | 25 | 20 |
| 32 | 古典自由主义 | 30 | 40 | 80 | 50 | 40 |
| 33 | 新自由主义 | 20 | 20 | 60 | 40 | 30 |
| 34 | 进步主义 | 60 | 20 | 75 | 10 | 15 |
| 35 | 绿色自由主义 | 50 | 30 | 70 | 20 | 20 |
| 36 | 激进主义 | 60 | 30 | 70 | 30 | 30 |
| 37 | 同情心自由意志主义 | 40 | 30 | 90 | 30 | 30 |
| 38 | 公民民族主义 | 50 | 70 | 70 | 40 | 40 |
| 39 | 全球主义 | 40 | 0 | 60 | 30 | 20 |
| 40 | 自由女性主义 | 50 | 30 | 75 | 10 | 20 |
| 41 | 技术自由主义 | 40 | 30 | 70 | 0 | 5 |
| 42 | 第三条道路 | 45 | 40 | 55 | 40 | 35 |
| 43 | 秩序自由主义 | 40 | 50 | 60 | 50 | 40 |
| 44 | 改良主义 | 60 | 40 | 60 | 40 | 35 |
| 45 | 社会自由意志主义 | 65 | 30 | 85 | 15 | 15 |

**Category 4: 中间派与融合政治**
| ID | Name | E | D | S | So | T |
|----|------|---|---|---|----|---|
| 46 | 中立主义 | 50 | 50 | 50 | 50 | 50 |
| 47 | 绝对中心 | 50 | 50 | 50 | 50 | 50 |
| 48 | 激进中间主义 | 50 | 40 | 60 | 40 | 40 |
| 49 | 基督教民主主义 | 55 | 40 | 50 | 70 | 60 |
| 50 | 分配主义 | 60 | 60 | 50 | 80 | 60 |
| 51 | 社群主义 | 55 | 60 | 40 | 70 | 50 |
| 52 | 家长制保守主义 | 55 | 60 | 30 | 75 | 60 |
| 53 | 混合政治 | 50 | 50 | 40 | 60 | 50 |
| 54 | 凯末尔主义 | 40 | 80 | 40 | 30 | 40 |
| 55 | 三民主义 | 60 | 75 | 50 | 60 | 50 |

**Category 5: 保守主义与右翼秩序派**
| ID | Name | E | D | S | So | T |
|----|------|---|---|---|----|---|
| 56 | 传统保守主义 | 40 | 70 | 40 | 85 | 65 |
| 57 | 新保守主义 | 30 | 80 | 50 | 70 | 60 |
| 58 | 旧保守主义 | 35 | 90 | 60 | 90 | 65 |
| 59 | 财政保守主义 | 20 | 50 | 60 | 60 | 50 |
| 60 | 民族保守主义 | 40 | 85 | 30 | 85 | 60 |
| 61 | 右翼民粹主义 | 45 | 90 | 40 | 80 | 60 |
| 62 | 神学保守主义 | 40 | 70 | 20 | 95 | 70 |
| 63 | 反动主义 | 30 | 80 | 10 | 100 | 70 |
| 64 | 君主专制 | 30 | 80 | 0 | 100 | 70 |
| 65 | 立宪君主制 | 40 | 60 | 50 | 75 | 60 |
| 66 | 自由意志保守主义 | 20 | 60 | 75 | 80 | 60 |
| 67 | 皮诺切特主义 | 10 | 80 | 5 | 80 | 60 |
| 68 | 财阀统治 | 0 | 20 | 30 | 50 | 40 |
| 69 | 财阀政治 | 0 | 40 | 40 | 60 | 45 |
| 70 | 农业主义 | 50 | 60 | 50 | 85 | 65 |

**Category 6: 自由意志主义与无政府资本主义**
| ID | Name | E | D | S | So | T |
|----|------|---|---|---|----|---|
| 71 | 右翼自由意志主义 | 15 | 40 | 90 | 50 | 40 |
| 72 | 无政府资本主义 | 0 | 20 | 100 | 50 | 30 |
| 73 | 小政府主义 | 10 | 30 | 90 | 50 | 40 |
| 74 | 阿哥拉主义 | 10 | 10 | 100 | 50 | 20 |
| 75 | 自愿主义 | 10 | 20 | 100 | 50 | 30 |
| 76 | 旧自由意志主义 | 10 | 70 | 90 | 85 | 60 |
| 77 | 客观主义 | 0 | 40 | 85 | 40 | 30 |
| 78 | 自由放任资本主义 | 0 | 30 | 70 | 50 | 40 |
| 79 | 霍普主义 | 0 | 80 | 95 | 90 | 60 |
| 80 | 个人无政府主义 | 30 | 10 | 100 | 40 | 20 |
| 81 | 地政自由意志主义 | 40 | 20 | 90 | 40 | 30 |
| 82 | 杰斐逊主义 | 50 | 60 | 70 | 50 | 45 |
| 83 | 市场无政府主义 | 20 | 10 | 100 | 40 | 20 |
| 84 | 粉红资本主义 | 0 | 20 | 80 | 0 | 10 |
| 85 | 密码无政府主义 | 10 | 0 | 100 | 20 | 0 |

**Category 7: 法西斯主义与极端威权**
| ID | Name | E | D | S | So | T |
|----|------|---|---|---|----|---|
| 86 | 法西斯主义 | 50 | 100 | 0 | 90 | 75 |
| 87 | 纳粹主义 | 40 | 100 | 0 | 90 | 75 |
| 88 | 教权法西斯主义 | 50 | 90 | 0 | 100 | 80 |
| 89 | 长枪党主义 | 60 | 90 | 0 | 95 | 75 |
| 90 | 斯特拉瑟主义 | 70 | 100 | 0 | 80 | 70 |
| 91 | 整合主义 | 55 | 95 | 5 | 95 | 75 |
| 92 | 新法西斯主义 | 45 | 95 | 5 | 85 | 70 |
| 93 | 基督法西斯主义 | 40 | 90 | 0 | 100 | 80 |
| 94 | 极权主义 | 50 | 80 | 0 | 50 | 60 |
| 95 | 国家资本主义 | 20 | 70 | 10 | 60 | 50 |

**Category 8: 边缘理论与缝合型向量**
| ID | Name | E | D | S | So | T |
|----|------|---|---|---|----|---|
| 96 | 生态法西斯主义 | 50 | 90 | 0 | 10 | 30 |
| 97 | 无政府虚无主义 | 50 | 0 | 100 | 0 | 10 |
| 98 | 无政府原始主义 | 80 | 0 | 100 | 100 | 90 |
| 99 | 超人类主义 | 50 | 20 | 70 | 0 | 0 |
| 100 | 技术官僚主义 | 60 | 40 | 10 | 10 | 5 |

- [ ] **Step 2: Save ideologies.json**

```bash
git add ideologies.json
```

---

### Task 3: Create `index.html` — HTML structure + CDN imports

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create HTML skeleton with Tailwind + Chart.js CDN, three-page structure**

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>意识形态测试</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    /* Custom avant-garde CSS goes here */
  </style>
</head>
<body class="bg-[#0f0c29] text-white overflow-x-hidden">
  <!-- Page 1: Home -->
  <div id="page-home" class="page active">
    <!-- Content -->
  </div>

  <!-- Page 2: Quiz -->
  <div id="page-quiz" class="page hidden">
    <!-- Content -->
  </div>

  <!-- Page 3: Results -->
  <div id="page-results" class="page hidden">
    <!-- Content -->
  </div>

  <!-- Canvas Background -->
  <canvas id="bg-canvas"></canvas>

  <!-- CRT Scan Overlay -->
  <div id="scan-overlay"></div>

  <script>
    // All JS logic here
  </script>
</body>
</html>
```

- [ ] **Step 2: Implement each page's HTML content**

**Home page HTML:**
```html
<div id="page-home" class="page active fixed inset-0 z-10 flex flex-col items-center justify-center">
  <canvas id="bg-canvas" class="fixed inset-0 z-0"></canvas>
  <div class="relative z-10 text-center px-4">
    <h1 id="glitch-title" class="text-6xl md:text-8xl font-black tracking-tighter mb-4"
        data-text="意识形态测试">
      意识形态测试
    </h1>
    <p class="text-lg md:text-xl text-[#8888aa] mb-8 font-mono">
      100 题 · 5 维坐标 · 精准政治光谱定位
    </p>
    <button id="btn-start"
      class="bg-[#ff2a75] text-white text-2xl font-bold py-4 px-12 border-2 border-black
             shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] hover:shadow-[8px_8px_0px_0px_rgba(0,0,0,1)]
             hover:-translate-y-1 hover:-translate-x-1 transition-all duration-200
             active:translate-y-1 active:translate-x-1 active:shadow-none">
      开始测试
    </button>
    <p class="mt-8 text-sm text-[#666688] max-w-md font-mono">
      本测试包含可能具有冒犯性的政治观点表述。<br>
      请根据你的「应然」判断作答。
    </p>
  </div>
  <div id="scan-overlay" class="fixed inset-0 pointer-events-none z-20 opacity-20"
       style="background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.3) 2px, rgba(0,0,0,0.3) 4px);">
  </div>
</div>
```

**Quiz page HTML:**
```html
<div id="page-quiz" class="page hidden fixed inset-0 z-10 flex flex-col">
  <!-- Progress bar -->
  <div class="relative z-10 w-full pt-6 px-6">
    <div class="flex justify-between text-sm text-[#8888aa] font-mono mb-2">
      <span id="progress-label">1 / 145</span>
      <span id="progress-pct">0%</span>
    </div>
    <div class="w-full h-2 bg-[#1a1a3e] border-2 border-black">
      <div id="progress-bar" class="h-full bg-[#ffd700] transition-all duration-300" style="width: 0%"></div>
    </div>
  </div>

  <!-- Question card -->
  <div class="relative z-10 flex-1 flex items-center justify-center px-6">
    <div id="question-card" class="w-full max-w-3xl bg-[#1a1a3e] border-2 border-black p-8 md:p-12
                shadow-[8px_8px_0px_0px_rgba(0,0,0,1)]">
      <div class="text-[#ff2a75] font-mono text-sm mb-4" id="q-number">14 / 145</div>
      <p class="text-xl md:text-2xl font-bold leading-relaxed mb-8" id="q-text">
        题目内容...
      </p>
      <!-- Likert buttons -->
      <div class="grid grid-cols-5 gap-3" id="likert-buttons">
        <button class="likert-btn bg-[#ff0040] hover:bg-[#cc0033] ..." data-value="1.0">非常同意</button>
        <button class="likert-btn bg-[#ff6640] hover:bg-[#cc5233] ..." data-value="0.5">同意</button>
        <button class="likert-btn bg-[#666688] hover:bg-[#555577] ..." data-value="0">中立</button>
        <button class="likert-btn bg-[#40aaff] hover:bg-[#3388cc] ..." data-value="-0.5">不同意</button>
        <button class="likert-btn bg-[#0040ff] hover:bg-[#0033cc] ..." data-value="-1.0">非常不同意</button>
      </div>
    </div>
  </div>

  <!-- Navigation -->
  <div class="relative z-10 w-full pb-8 px-6 flex justify-between">
    <button id="btn-prev" class="font-mono text-[#8888aa] hover:text-white transition-colors px-4 py-2 border border-[#8888aa]">
      ← 上一题
    </button>
    <button id="btn-next" class="font-mono text-white bg-[#00d4ff] px-6 py-2 border-2 border-black
                shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] hover:shadow-[6px_6px_0px_0px_rgba(0,0,0,1)] transition-all">
      下一题 →
    </button>
  </div>
</div>
```

**Results page HTML:**
```html
<div id="page-results" class="page hidden fixed inset-0 z-10 flex flex-col items-center justify-center overflow-y-auto">
  <div class="relative z-10 max-w-5xl w-full px-6 py-12">
    <h2 class="text-4xl md:text-5xl font-black text-center mb-8">你的政治坐标</h2>
    <!-- Radar chart container -->
    <div class="bg-[#1a1a3e] border-2 border-black p-4 md:p-8 mb-8
                shadow-[8px_8px_0px_0px_rgba(0,0,0,1)]">
      <canvas id="radar-chart" class="max-h-[500px] w-full"></canvas>
    </div>
    <!-- Best match -->
    <div id="match-card" class="bg-[#1a1a3e] border-2 border-black p-8 mb-6
                shadow-[8px_8px_0px_0px_rgba(0,0,0,1)] text-center">
      <p class="text-sm font-mono text-[#8888aa] mb-2">最佳匹配</p>
      <h3 id="match-name" class="text-3xl md:text-4xl font-black text-[#ff2a75] mb-2"></h3>
      <p id="match-name-en" class="text-lg font-mono text-[#00d4ff] mb-4"></p>
      <p id="match-desc" class="text-base text-gray-300 max-w-2xl mx-auto leading-relaxed"></p>
    </div>
    <!-- Axis scores -->
    <div id="axis-scores" class="grid grid-cols-2 md:grid-cols-5 gap-4 mb-8"></div>
    <!-- Top 3 -->
    <div id="top3" class="text-center mb-8"></div>
    <!-- Restart -->
    <div class="text-center">
      <button id="btn-restart" class="bg-[#ffd700] text-black font-bold py-3 px-8 border-2 border-black
                  shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] hover:shadow-[8px_8px_0px_0px_rgba(0,0,0,1)]
                  transition-all">重新测试</button>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Implement complete avant-garde CSS**

Key CSS classes:
```css
.page { transition: opacity 0.4s ease; }
.page.hidden { display: none; opacity: 0; }
.page.active { display: flex; opacity: 1; }

/* Glitch effect */
@keyframes glitch {
  0% { transform: translate(0); text-shadow: none; }
  20% { transform: translate(-2px, 2px); text-shadow: 2px 0 #ff2a75, -2px 0 #00d4ff; }
  40% { transform: translate(2px, -1px); text-shadow: -2px 0 #ff2a75, 2px 0 #00d4ff; }
  60% { transform: translate(-1px, 1px); text-shadow: 1px 0 #39ff14, -1px 0 #ff0040; }
  80% { transform: translate(1px, -2px); text-shadow: -1px 0 #ff2a75, 1px 0 #00d4ff; }
  100% { transform: translate(0); text-shadow: none; }
}
.glitch { animation: glitch 0.3s ease; }

/* Likert button base */
.likert-btn {
  @apply text-white font-bold py-3 px-2 border-2 border-black text-sm
         shadow-[3px_3px_0px_0px_rgba(0,0,0,1)]
         transition-all duration-150 hover:-translate-y-0.5 hover:-translate-x-0.5
         active:translate-y-0.5 active:translate-x-0.5 active:shadow-none;
}

/* Card transition */
.fault-out {
  animation: faultOut 0.3s ease forwards;
}
.fault-in {
  animation: faultIn 0.3s ease forwards;
}
@keyframes faultOut {
  0% { transform: translateX(0); opacity: 1; clip-path: inset(0); }
  50% { transform: translateX(10px); opacity: 0.7; clip-path: inset(10% 0 30% 0); }
  100% { transform: translateX(-50px); opacity: 0; clip-path: inset(50% 0 0 0); }
}
@keyframes faultIn {
  0% { transform: translateX(50px); opacity: 0; clip-path: inset(0 0 50% 0); }
  50% { transform: translateX(-10px); opacity: 0.7; clip-path: inset(30% 0 10% 0); }
  100% { transform: translateX(0); opacity: 1; clip-path: inset(0); }
}

/* Typewriter cursor */
.typewriter::after {
  content: '|';
  animation: blink 1s step-end infinite;
}
@keyframes blink { 50% { opacity: 0; } }

/* Pulse glow */
@keyframes pulseGlow {
  0%, 100% { box-shadow: 0 0 5px #ffd700; }
  50% { box-shadow: 0 0 20px #ffd700, 0 0 40px #ffd70088; }
}
```

---

### Task 4: Implement Canvas dynamic background

**Files:**
- Modify: `index.html` (append to JS in `<script>`)

- [ ] **Step 1: Write the Canvas background animation**

```javascript
const canvas = document.getElementById('bg-canvas');
const ctx = canvas.getContext('2d');
let W, H;

function resizeCanvas() {
  W = canvas.width = window.innerWidth;
  H = canvas.height = window.innerHeight;
}
window.addEventListener('resize', resizeCanvas);
resizeCanvas();

const shapes = Array.from({ length: 12 }, () => ({
  x: Math.random() * W,
  y: Math.random() * H,
  vx: (Math.random() - 0.5) * 0.5,
  vy: (Math.random() - 0.5) * 0.5,
  size: 20 + Math.random() * 60,
  type: Math.random() > 0.5 ? 'circle' : 'triangle',
  hue: Math.random() * 360,
  rot: Math.random() * Math.PI * 2,
  rotSpeed: (Math.random() - 0.5) * 0.01,
}));

let mouseX = 0, mouseY = 0;
window.addEventListener('mousemove', e => { mouseX = e.clientX; mouseY = e.clientY; });

function drawBg() {
  ctx.clearRect(0, 0, W, H);

  // Dark gradient base
  const grad = ctx.createRadialGradient(W/2, H/2, 0, W/2, H/2, Math.max(W,H)*0.7);
  grad.addColorStop(0, '#151035');
  grad.addColorStop(1, '#0f0c29');
  ctx.fillStyle = grad;
  ctx.fillRect(0, 0, W, H);

  // Mouse parallax offset
  const px = (mouseX / W - 0.5) * 20;
  const py = (mouseY / H - 0.5) * 20;

  for (const s of shapes) {
    s.x += s.vx;
    s.y += s.vy;
    s.rot += s.rotSpeed;
    if (s.x < -100) s.x = W + 100;
    if (s.x > W + 100) s.x = -100;
    if (s.y < -100) s.y = H + 100;
    if (s.y > H + 100) s.y = -100;

    ctx.save();
    ctx.translate(s.x + px * 0.1, s.y + py * 0.1);
    ctx.rotate(s.rot);
    ctx.globalAlpha = 0.15 + 0.1 * Math.sin(Date.now() * 0.001 + s.hue);
    ctx.strokeStyle = `hsl(${s.hue + Date.now() * 0.02}, 80%, 60%)`;
    ctx.lineWidth = 2;

    if (s.type === 'circle') {
      ctx.beginPath();
      ctx.arc(0, 0, s.size / 2, 0, Math.PI * 2);
      ctx.stroke();
    } else {
      ctx.beginPath();
      for (let i = 0; i < 3; i++) {
        const a = (Math.PI * 2 / 3) * i - Math.PI / 2;
        const px = Math.cos(a) * s.size / 2;
        const py = Math.sin(a) * s.size / 2;
        i === 0 ? ctx.moveTo(px, py) : ctx.lineTo(px, py);
      }
      ctx.closePath();
      ctx.stroke();
    }
    ctx.restore();
  }
  requestAnimationFrame(drawBg);
}
drawBg();
```

---

### Task 5: Implement quiz logic, scoring, and state management

**Files:**
- Modify: `index.html` (JS section)

- [ ] **Step 1: Implement data loading, state, and scoring**

```javascript
// State
const state = {
  questions: [],
  ideologies: [],
  currentIdx: 0,
  scores: { econ: 0, diplomatic: 0, state: 0, society: 0, tech: 0 },
  maxScores: { econ: 0, diplomatic: 0, state: 0, society: 0, tech: 0 },
  answered: {}, // { [questionId]: multiplier }
  totalQuestions: 0,
};

// Load data
async function loadData() {
  const [qRes, iRes] = await Promise.all([
    fetch('questions.json'),
    fetch('ideologies.json')
  ]);
  state.questions = await qRes.json();
  state.ideologies = await iRes.json();
  state.totalQuestions = state.questions.length;

  // Pre-calculate max scores for normalization
  for (const q of state.questions) {
    for (const axis of ['econ', 'diplomatic', 'state', 'society', 'tech']) {
      state.maxScores[axis] += Math.abs(q.weights[axis]);
    }
  }
}

// Normalize scores to -100..100
function normalizeScores(raw) {
  const norm = {};
  for (const axis of ['econ', 'diplomatic', 'state', 'society', 'tech']) {
    const m = state.maxScores[axis];
    norm[axis] = m > 0 ? Math.round((raw[axis] / m) * 100) : 0;
  }
  return norm;
}

// Answer a question
function answerQuestion(id, multiplier) {
  state.answered[id] = multiplier;
  const q = state.questions.find(q => q.id === id);
  for (const axis of ['econ', 'diplomatic', 'state', 'society', 'tech']) {
    state.scores[axis] += q.weights[axis] * multiplier;
  }
}
```

- [ ] **Step 2: Implement question navigation**

```javascript
function renderQuestion(index) {
  const q = state.questions[index];
  document.getElementById('q-number').textContent = `${q.id} / ${state.totalQuestions}`;
  document.getElementById('q-text').textContent = q.text;
  document.getElementById('progress-label').textContent = `${q.id} / ${state.totalQuestions}`;
  const pct = Math.round(((q.id - 1) / state.totalQuestions) * 100);
  document.getElementById('progress-pct').textContent = `${pct}%`;
  document.getElementById('progress-bar').style.width = `${pct}%`;

  // Highlight selected answer if already answered
  const prevAnswer = state.answered[q.id];
  document.querySelectorAll('.likert-btn').forEach(btn => {
    const val = parseFloat(btn.dataset.value);
    btn.classList.toggle('ring-2', val === prevAnswer);
    btn.classList.toggle('ring-[#ffd700]', val === prevAnswer);
  });

  // Update button visibility
  document.getElementById('btn-prev').style.visibility = index === 0 ? 'hidden' : 'visible';
  document.getElementById('btn-next').textContent = index === state.totalQuestions - 1
    ? '查看结果 →' : '下一题 →';
}

function goNext() {
  const q = state.questions[state.currentIdx];
  if (state.answered[q.id] === undefined) return; // Must answer

  if (state.currentIdx < state.totalQuestions - 1) {
    // Fault transition
    const card = document.getElementById('question-card');
    card.classList.add('fault-out');
    setTimeout(() => {
      state.currentIdx++;
      renderQuestion(state.currentIdx);
      card.classList.remove('fault-out');
      card.classList.add('fault-in');
      setTimeout(() => card.classList.remove('fault-in'), 300);
    }, 300);
  } else {
    showResults();
  }
}

function goPrev() {
  if (state.currentIdx > 0) {
    state.currentIdx--;
    renderQuestion(state.currentIdx);
  }
}

// Likert button handlers
document.querySelectorAll('.likert-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    const multiplier = parseFloat(btn.dataset.value);
    const q = state.questions[state.currentIdx];
    // Remove previous answer for this question
    if (state.answered[q.id] !== undefined) {
      const prevMult = state.answered[q.id];
      for (const axis of ['econ', 'diplomatic', 'state', 'society', 'tech']) {
        state.scores[axis] -= q.weights[axis] * prevMult;
      }
    }
    answerQuestion(q.id, multiplier);
    renderQuestion(state.currentIdx);
  });
});

document.getElementById('btn-next').addEventListener('click', goNext);
document.getElementById('btn-prev').addEventListener('click', goPrev);
```

---

### Task 6: Implement results page with radar chart and ideology matching

**Files:**
- Modify: `index.html` (JS section)

- [ ] **Step 1: Implement ideology matching algorithm (weighted Euclidean distance)**

```javascript
function findMatches(normalizedScores) {
  const results = state.ideologies.map(ideo => {
    let distSq = 0;
    for (const axis of ['econ', 'diplomatic', 'state', 'society', 'tech']) {
      distSq += Math.pow(normalizedScores[axis] - ideo.coords[axis], 2);
    }
    return { ...ideo, distance: Math.sqrt(distSq) };
  });
  results.sort((a, b) => a.distance - b.distance);
  return results;
}
```

- [ ] **Step 2: Render Chart.js radar chart with avant-garde styling**

```javascript
function renderRadarChart(normalized) {
  const ctx = document.getElementById('radar-chart').getContext('2d');

  const axisLabels = {
    econ: { high: '平等', low: '市场' },
    diplomatic: { high: '国家', low: '全球' },
    state: { high: '权威', low: '自由' },
    society: { high: '传统', low: '进步' },
    tech: { high: '技术控制', low: '技术加速' }
  };

  const labels = Object.entries(axisLabels).map(([key, v]) =>
    `${v.high}\n${v.low}`
  );

  new Chart(ctx, {
    type: 'radar',
    data: {
      labels,
      datasets: [{
        label: '你的政治坐标',
        data: Object.values(normalized),
        backgroundColor: 'rgba(255, 42, 117, 0.15)',
        borderColor: '#ff2a75',
        borderWidth: 3,
        pointBackgroundColor: '#ff2a75',
        pointBorderColor: '#fff',
        pointBorderWidth: 2,
        pointRadius: 5,
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: true,
      animation: {
        duration: 2000,
        easing: 'easeOutQuart'
      },
      scales: {
        r: {
          min: -100,
          max: 100,
          ticks: {
            stepSize: 50,
            color: '#8888aa',
            backdropColor: 'transparent',
            font: { family: 'monospace', size: 10 }
          },
          grid: { color: 'rgba(136, 136, 170, 0.2)' },
          angleLines: { color: 'rgba(136, 136, 170, 0.2)' },
          pointLabels: {
            color: '#ffffff',
            font: { family: 'monospace', size: 11, weight: 'bold' }
          }
        }
      },
      plugins: {
        legend: { display: false }
      }
    }
  });
}
```

- [ ] **Step 3: Implement results display (typewriter, top 3, axis scores)**

```javascript
function showResults() {
  const normalized = normalizeScores(state.scores);
  document.getElementById('page-quiz').classList.add('hidden');
  document.getElementById('page-quiz').classList.remove('active');
  document.getElementById('page-results').classList.remove('hidden');
  document.getElementById('page-results').classList.add('active');

  renderRadarChart(normalized);

  const matches = findMatches(normalized);
  const best = matches[0];

  // Typewriter effect for name
  const nameEl = document.getElementById('match-name');
  const fullName = best.name;
  nameEl.textContent = '';
  let charIdx = 0;
  const twInterval = setInterval(() => {
    nameEl.textContent += fullName[charIdx];
    charIdx++;
    if (charIdx >= fullName.length) clearInterval(twInterval);
  }, 80);

  document.getElementById('match-name-en').textContent = best.nameEn;
  document.getElementById('match-desc').textContent = best.description;

  // Axis scores
  const axisLabels = {
    econ: ['经济', '平等', '市场'],
    diplomatic: ['外交', '国家', '全球'],
    state: ['国家机构', '权威', '自由'],
    society: ['社会文化', '传统', '进步'],
    tech: ['科技', '技术控制', '技术加速']
  };
  const container = document.getElementById('axis-scores');
  container.innerHTML = '';
  for (const [key, [label, high, low]] of Object.entries(axisLabels)) {
    const val = normalized[key];
    const pct = (val + 100) / 2; // -100..100 -> 0..100
    const div = document.createElement('div');
    div.className = 'bg-[#1a1a3e] border-2 border-black p-4 text-center shadow-[4px_4px_0px_0px_rgba(0,0,0,1)]';
    div.innerHTML = `
      <div class="text-sm font-mono text-[#8888aa] mb-1">${label}</div>
      <div class="text-2xl font-black mb-1" style="color: ${val >= 0 ? '#ff2a75' : '#00d4ff'}">${val >= 0 ? '+' : ''}${val}</div>
      <div class="flex justify-between text-xs font-mono text-[#666688]">
        <span>${low}</span>
        <span>${high}</span>
      </div>
      <div class="w-full h-1.5 bg-[#0f0c29] mt-1 border border-black">
        <div class="h-full" style="width: ${pct}%; background: ${val >= 0 ? '#ff2a75' : '#00d4ff'}"></div>
      </div>
    `;
    container.appendChild(div);
  }

  // Top 3
  const top3El = document.getElementById('top3');
  top3El.innerHTML = '<h4 class="text-lg font-mono text-[#8888aa] mb-4">其他匹配</h4>';
  const list = document.createElement('div');
  list.className = 'flex flex-wrap justify-center gap-3';
  for (let i = 1; i < 3; i++) {
    const m = matches[i];
    const span = document.createElement('span');
    span.className = 'bg-[#1a1a3e] border-2 border-black px-4 py-2 text-sm font-mono shadow-[3px_3px_0px_0px_rgba(0,0,0,1)]';
    span.textContent = `${i+1}. ${m.name}`;
    list.appendChild(span);
  }
  top3El.appendChild(list);
}

// Restart
document.getElementById('btn-restart').addEventListener('click', () => {
  state.currentIdx = 0;
  state.scores = { econ: 0, diplomatic: 0, state: 0, society: 0, tech: 0 };
  state.answered = {};
  document.getElementById('page-results').classList.add('hidden');
  document.getElementById('page-results').classList.remove('active');
  document.getElementById('page-home').classList.remove('hidden');
  document.getElementById('page-home').classList.add('active');
});
```

---

### Task 7: Wire up page transitions and event listeners

**Files:**
- Modify: `index.html` (JS section — initialization)

- [ ] **Step 1: App initialization with page transitions**

```javascript
// Start button
document.getElementById('btn-start').addEventListener('click', async () => {
  document.getElementById('page-home').classList.add('hidden');
  document.getElementById('page-home').classList.remove('active');
  document.getElementById('page-quiz').classList.remove('hidden');
  document.getElementById('page-quiz').classList.add('active');
  renderQuestion(state.currentIdx);
});

// Periodic glitch effect on title
setInterval(() => {
  const title = document.getElementById('glitch-title');
  if (title && document.getElementById('page-home').classList.contains('active')) {
    title.classList.add('glitch');
    setTimeout(() => title.classList.remove('glitch'), 300);
  }
}, 4000);

// Initialize on load
document.addEventListener('DOMContentLoaded', async () => {
  await loadData();
  // Show home page
  document.getElementById('page-home').classList.add('active');
});
```

---

### Task 8: Create GitHub Actions workflow

**Files:**
- Create: `.github/workflows/pages.yml`

- [ ] **Step 1: Create pages.yml**

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ "main" ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 2: Verify workflow file**

Run: `cat .github/workflows/pages.yml | head -5`
Expected: `name: Deploy to GitHub Pages`

---

### Task 9: Initialize git and commit all files

- [ ] **Step 1: Git status check**

Run: `git status`
Expected: untracked files including index.html, questions.json, ideologies.json, .github/

- [ ] **Step 2: Stage and commit**

```bash
git add index.html questions.json ideologies.json .github/workflows/pages.yml docs/superpowers/specs/2026-05-24-ideological-test-design.md
git commit -m "feat: ideological test SPA with 140+ questions, 5-axis scoring, 100 ideologies, avant-garde UI"
```

- [ ] **Step 3: Verify commit**

Run: `git log --oneline`
Expected: commit with the message above

---

## Spec Coverage Check

| Spec Requirement | Task |
|---|---|
| 100 ideologies in results | Task 2 |
| ~140-150 questions with 5-axis weights | Task 1 |
| CNValues question integration | Task 1 (Step 2) |
| Multi-axis weight assignment | Task 1 (Step 1) |
| Avant-garde visual design | Task 3, Task 4 |
| Canvas dynamic background | Task 4 |
| CSS glitch/fault animations | Task 3 (Step 3) |
| CRT scan overlay | Task 3 (Step 2) |
| Likert 5-point scale | Task 5 |
| Progress bar | Task 5 |
| Weighted Euclidean distance matching | Task 6 |
| Chart.js radar chart | Task 6 |
| Top 3 ideology matches | Task 6 (Step 3) |
| Typewriter effect on results | Task 6 (Step 3) |
| GitHub Pages deployment | Task 8 |
| Single HTML file, zero build | Task 3 |
