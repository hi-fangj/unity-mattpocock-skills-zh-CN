# Logic Prototype

一个自包含的 HTML 文件（**shareable demo**）：让任何人通过点击按钮驱动 state model。用于问题围绕 **business logic、state transitions 或 data shape** 的场景，也就是纸面上看起来合理，但只有跑过真实 cases 才会感觉哪里不对的东西。

因为它是一个无需安装的文件，你可以把它交给非开发者（designer、PM、domain expert），让他们亲自感受这个 model。所以它要说他们的语言，而不是代码的语言。

## When this is the right shape

- "I'm not sure if this state machine handles the edge case where X then Y."
- "Does this data model actually let me represent the case where..."
- "I want to feel out what the API should look like before writing it."
- 任何有人想**按按钮并观察 state 变化**的情况。

如果问题是 “what should this look like”，这是错误分支。使用 [UI.md](UI.md)。

## Process

### 1. State the question

写代码前，先写下你正在 prototype 哪个 state model、回答什么问题。一段即可，放在 demo 顶部（可见的 intro，不只是 comment）。回答错问题的 logic prototype 纯属浪费；把问题显式写出来，这样无论用户现在旁观，还是之后 AFK 回来看，都能检查。

### 2. Isolate the logic in a portable module

把真正的 logic，也就是回答问题的那部分，放在单个 `<script>` block 里，写成一个小而纯粹的 module，让它之后可以被拿出来放进真实 codebase。页面是 throwaway；这个 module 不应该是。

合适形状取决于问题：

- **Pure reducer**：`(state, action) => state`。适合 actions 是离散 events、state 是单个值的场景。
- **State machine**：显式 states 和 transitions。适合 “现在到底哪些 actions 合法” 本身就是问题的一部分。
- **一组作用于 plain data type 的 pure functions**。适合没有隐式 current state，只有 transformations 的场景。
- **Class 或带清晰 method surface 的 module**，当 logic 确实拥有持续的 internal state。

选择最适合问题的形状，而不是最容易接到页面上的形状。保持 pure：不要 DOM、不要 `document`、不要让 button handlers 伸手进入它内部。页面调用它；没有东西反向流动。这让 prototype 在自身生命周期之后仍有价值：问题被回答后，验证过的 reducer / machine / function set 会自己搬进真实 module。

### 3. Build the shareable HTML file

一个文件，纯 HTML/CSS/JS：没有 framework、没有 bundler、没有 server，一切都 inline，所以双击就能打开，经得起被邮件传来传去。任何人都应该能通过打开它来运行。

为 non-developer 编写。每个 label 都用 **domain language**，而不是代码：buttons 和 state 读起来像业务，而不是 reducer。用平实的语言解释正在发生什么。

自上而下用清晰的分层排版：

1. **标题和一行说明**：这个 demo 让你探索什么（step 1 的问题）。
2. **Current state**：完整相关 state，渲染为可读面板（带 label 的 fields，不是 raw JSON dump），每次点击后重新渲染，让变化可见。在有助于 non-developer 跟进的地方，指出刚刚发生了什么变化。
3. **Free-play buttons**：每个 action 一个 button，始终可用，所以任何人都能按任意顺序戳 model。每次点击 dispatch 其 action 并重新渲染 state。
4. **Guided walkthroughs**：一组 **scenarios**，每个 tab 一个。每个 tab 有一段简短的平实语言描述（它搭建的场景以及该观察什么），下面是对应 scenario 的有序 **buttons to press**。每一步都是真实 button：点击它执行该 action 并进入下一步。启动 walkthrough 会重置到已知初始 state，这样 scenario 每次都按同样方式运行。

选择能演示 awkward cases 的 scenarios（happy path、棘手的 edge case、一次对"本该非法"的尝试），也就是纸面上难以推理的那些。

保持美观但克制：干净的 typography、充裕的 spacing、一个 accent colour。不要 animations、不要 gimmicks：不要让任何东西和 state 与 buttons 竞争。

### 4. Hand it over

把文件发给他们，或替他们打开。他们会随时点击 walkthroughs 和 free-play；真正有趣的时刻是他们说 "wait, that shouldn't be possible" 或 "huh, I assumed X would be different" 的时候。这些是*想法*里的 bug，也正是 prototype 的目的。如果他们想添加新 actions 或新 scenario，就添加。Prototypes 会演进。

### 5. Capture the answer and the prototype

Prototype 回答问题后，capture answer，再按 [SKILL](SKILL.md) 描述的方式 capture prototype。Logic-specific mapping：验证过的 reducer / machine / function set 搬进真实 module（吸收 decision）；HTML shell 跟随 prototype 留在作为 primary source 的 throwaway branch，而且因为它是单个自包含文件，在那里它依然 trivial 地可重新运行。

## Anti-patterns

- **不要加 tests。** 需要 tests 的 prototype 已经不再是 prototype。
- **不要接真实 database。** 除非问题专门关于 persistence，否则使用 in-memory state。
- **不要 generalise。** 不要做 "what if we wanted to support X later"。Prototype 回答一个问题。
- **不要把 logic 和 page 混在一起。** 如果 pure module 引用了 DOM、`document` 或 button handlers，它就不再 liftable。让 page 作为 pure module 外面的薄 shell。
- **不要引入 framework、bundler 或 server。** 一个收件人双击打开的文件；React app 或 dev server 会破坏 "shareable"。
- **不要把 HTML shell 发到 production。** Page 是为手动点击优化。它背后的 logic module 才是值得保留的部分。
