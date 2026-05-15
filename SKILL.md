---
name: miniprogram-vibe-coding-starter
description: This skill should be used when a beginner, product manager, creator, operator, or non-engineer wants to build, iterate, launch, or maintain a WeChat Mini Program from 0 to 1 with AI-assisted Vibe Coding. It applies to mini-program MVP planning, requirements clarification, safe implementation, data modeling, UI iteration, cloud development decisions, launch preparation, review checklists, GitHub syncing, and post-launch feedback iteration.
agent_created: true
---

# Miniprogram Vibe Coding Starter

## Purpose

Use this skill to help non-engineers build and maintain a WeChat Mini Program MVP with AI-assisted Vibe Coding. Convert rough product ideas into safe, small, verifiable development steps. Prioritize product clarity, implementation safety, user confirmation, and maintainability.

Treat the human as the product owner. Avoid assuming technical knowledge. Explain tradeoffs in plain language and implement only after the user approves the plan.

## When to Use

Use this skill for requests such as:

- Build a WeChat Mini Program from scratch.
- Turn a product idea into a mini-program MVP.
- Add or modify mini-program features with AI assistance.
- Debug or polish mini-program pages, interactions, sharing, cloud functions, or data.
- Help a non-programmer organize requirements for Vibe Coding.
- Prepare a mini-program for review, release,备案, or user feedback collection.
- Maintain menus, lists, business data, local storage, or cloud data.
- Explain mini-program code/data structure to a beginner.

Do not use this skill for unrelated web apps, backend-only services, or non-WeChat products unless the user explicitly wants to adapt the Vibe Coding workflow.

## Core Rule: Plan First, Execute After Confirmation

Before changing code, style, configuration, data, storage, cloud resources, files, directories, or release settings:

1. Inspect relevant files if needed.
2. Provide a clear plan.
3. List the affected files and areas.
4. State what will not be changed.
5. Explain risks and rollback/verification approach.
6. Define acceptance criteria.
7. Wait for explicit user confirmation.

Use this format:

```text
目标：
改动范围：
不改什么：
风险点：
验收标准：
推荐方案：
```

Read-only analysis can happen before confirmation. Implementation cannot.

## Standard Vibe Coding Workflow

### 1. Clarify the product intent

Translate vague ideas into concrete product intent:

```text
用户是谁？
解决什么问题？
核心场景是什么？
最小可用版本必须包含什么？
哪些功能先不做？
```

For beginners, ask in product language instead of technical language.

### 2. Define the MVP path

Identify the shortest complete user journey. Example structure:

```text
首页 → 填/选信息 → 核心操作页 → 结果页 → 分享/保存/反馈
```

Keep the first version small. Avoid building advanced admin panels, complex account systems, or unnecessary permissions before proving the core experience.

### 3. Choose implementation strategy

For most beginner mini-program MVPs, prefer:

- WeChat native mini-program: WXML / WXSS / JavaScript.
- WeChat Cloud Development for lightweight backend needs.
- Local storage for personal/offline state.
- Cloud database only when cross-user sharing, feedback collection, or persistence is required.

Avoid adding frameworks, complex build systems, or heavy architecture unless the project clearly needs them.

### 4. Implement in small steps

Break work into small changes:

```text
页面结构 → 数据结构 → core logic → UI polish → validation → sharing → launch checks
```

After each change, verify the user path still works. Avoid broad rewrites.

### 5. Verify and summarize

After implementation:

- Run syntax checks for changed JS files.
- Run project-specific data checks if available.
- Test the core user path manually.
- Summarize changed files, what changed, checks run, and remaining risks.

## Recommended Mini Program Architecture

Use a simple structure for early MVPs:

```text
mini-program/
├── app.js                 # app entry, cloud init, global state entrypoints
├── app.json               # page registration and global settings
├── app.wxss               # global style tokens and shared classes
├── pages/
│   ├── index/             # landing / selection / search
│   ├── form-or-setup/     # configuration step if needed
│   ├── main/              # core operation page
│   ├── result/            # result / report page
│   └── profile/           # history / settings if needed
├── utils/
│   ├── config.js          # storage keys, env IDs, constants
│   ├── constants.js       # static business data
│   ├── helpers.js         # reusable pure helpers
│   └── share.js           # share menu and share title helpers
├── scripts/               # local-only check/import scripts
└── cloudfunctions/        # cloud functions, if needed
```

Keep page files focused:

- WXML: structure.
- WXSS: visual style.
- JS: state and business logic.
- JSON: page component/config declaration.

## State Management Rules

Avoid writing random global state everywhere. Define state entrypoints in `app.js` or a small store/helper module.

Good pattern:

```js
app.startSession(options)
app.resetSession()
app.updateCurrentSelection(data)
```

Bad pattern:

```js
app.globalData.foo = bar // scattered across pages without rules
```

Before implementing state logic, define boundaries:

```text
进入新一局是否清空旧数据？
返回上一页是否保留状态？
分享打开是否依赖本机状态？
历史记录存在本地还是云端？
```

For local-only state, do not deep-link share pages that require another user's local storage.

## Data Modeling for Beginners

Explain business data as simple tables.

Common data layers:

```text
主数据表：业务对象名片，如餐厅、课程、任务、商品
明细表：对应项目，如菜单、步骤、明细项、子任务
映射字段：把主数据和明细数据连起来的钥匙，如 categoryId、typeId、groupId
用户本地数据：用户自己新增/修改的数据
云端数据：需要跨用户、跨设备、审核/反馈/分享的数据
```

When ingesting real-world data:

1. Ask the user for screenshots, links, documents, or rough lists.
2. Use AI to extract and normalize the data first.
3. Output a table with confidence levels and uncertain fields.
4. Ask the user to confirm uncertain items.
5. Convert confirmed data to code/config/database format.
6. Run data checks.

Recommended intermediate table:

```text
名称 | 所属分组/档位 | 类型 | 估值/价格 | 置信度 | 备注
```

Never silently invent high-impact data. Mark uncertain data clearly.

## UI / UX Guidelines

For early mini-program MVPs:

- Prefer clean card layouts.
- Use rpx units.
- Add `box-sizing: border-box` globally.
- Keep buttons centered and easy to tap.
- Use modest shadows and consistent radius.
- Avoid overly saturated UI unless intentionally playful.
- Test on real phones, not only screenshots or devtools.

For scrollable category tabs, use `scroll-into-view` when a selected item may be partially hidden.

For dynamic tags/badges, ensure data fields are consistent across duplicated lists or categories.

## Sharing and WeChat Pitfalls

Do not assume that备案、审核通过、发布成功 automatically enables sharing. Each page that should support sharing needs explicit configuration:

```js
onShareAppMessage() {
  return {
    title: '分享标题',
    path: '/pages/index/index'
  };
}

onShareTimeline() {
  return {
    title: '朋友圈标题',
    query: ''
  };
}
```

Enable the top-right menu where needed:

```js
wx.showShareMenu({
  withShareTicket: true,
  menus: ['shareAppMessage', 'shareTimeline']
});
```

Wechat does not allow a normal page button to directly open the Moments publishing panel. Use a guide modal:

```text
请点击右上角“···”，选择“分享到朋友圈”。
```

For pages that depend on local state, share back to the homepage unless cloud share data is implemented.

## Privacy and Compliance Rules

Avoid new privacy permissions in early MVPs unless they are essential.

Be cautious with:

- Save to album.
- Location.
- Phone number.
- User profile.
- Camera/microphone.
- Contacts.

If a feature can be replaced with a lower-risk alternative, prefer the lower-risk route. Example: use built-in sharing instead of generating and saving posters to album.

For WeChat upload packages, exclude local-only resources when needed:

```text
scripts/
docs/
README.md
node_modules/
```

## Cloud Development Guidance

Use cloud development only when needed:

- Feedback submission.
- Wish lists / user requests.
- Cross-user shared detail pages.
- Persistent records across devices.

Client validation is not enough. Repeat important validation in cloud functions:

```text
trim input
check required fields
limit length
block obvious sensitive words
write createTime/status/openid when appropriate
```

After modifying cloud functions, remind the user to redeploy them in WeChat DevTools.

## GitHub for Non-Programmers

Prefer GitHub Desktop for beginners:

```text
1. Open GitHub Desktop
2. Add Local Repository
3. Review Changes
4. Write Summary
5. Commit to main
6. Push origin
```

Explain simply:

```text
Commit = 保存一个本地版本
Push = 上传到 GitHub
```

Recommend syncing after meaningful feature batches or before risky changes.

## Review Checklist

Before release or after meaningful changes, check:

```text
核心路径是否跑通？
页面是否能正常进入？
返回/重选是否清空或保留正确状态？
分享给朋友是否正常？
分享到朋友圈入口是否正常？
输入校验是否覆盖空值、超长、非法值？
云函数是否已部署？
本地检查脚本是否通过？
GitHub 是否已 commit + push？
README 是否记录了新的维护规则？
```

For code quality review, categorize findings:

```text
可安全改：重复代码、硬编码、无引用文件、小范围工具抽取
需确认后改：UI、交互流程、状态结构、数据结构、云函数
高风险不建议动：历史数据结构、已上线核心路径、大型重构、隐私权限
```

## Response Style

Use simple Chinese. Be practical and candid. Translate code concepts into metaphors when helpful:

```text
主数据 = 名片
明细数据 = 文件夹里的清单
映射字段 = 钥匙
Commit = 本地存档
Push = 上传云端
```

For non-engineer users, focus on what they need to decide and how to verify results, not on showing off technical details.
