# ChatGPT Pro Reviewer for Codex

一个调用网页版 ChatGPT Pro 的 Codex Skill。

## 中文

### 功能简介

这个 Codex Skill 会直接操作可见的 ChatGPT 网页会话：

- 自动或显式触发 Skill
- 打开一个空白 ChatGPT 对话
- 确认编辑框关联的模式显示为 `Pro`
- 输入并核对提示词
- 仅点击一次发送
- 等待回复稳定后，将结果带回 Codex

它使用用户现有的网页登录状态，不需要 API Key，也不调用私有接口。

### 验收结果

一次真实网页验收成功完成：

```text
CODEX_PRO_SESSION_OK

一个上线计划至少应明确系统稳定性异常、核心业务指标恶化、
数据安全或合规风险三类回滚条件。
```

验收过程中确认了以下行为：新建独立会话、编辑框显示 `Pro`、提示词仅发送一次、返回内容与测试问题匹配。

### 前置条件

- Codex Desktop，且支持可见浏览器的 computer-use 控制
- 已在 ChatGPT 网页登录
- 当前账号可使用 `Pro` intelligence 模式
- 可以访问 `https://chatgpt.com/`

这个 Skill 不会绕过订阅、登录、地区或账号权限限制。

### 安装

克隆或下载本仓库，然后在仓库根目录执行：

```bash
skill_dir="${CODEX_HOME:-$HOME/.codex}/skills/chatgpt-pro-reviewer"
mkdir -p "$skill_dir"
cp SKILL.md "$skill_dir/"
cp -R agents "$skill_dir/"
```

如果 Codex 没有立即识别，请新建一个 Codex 会话或重启 Codex。

### 使用方法

隐式调用，无需写 Skill 名称：

```text
请调用网页版 ChatGPT Pro，帮我找出这个上线方案最大的风险：
先运行单元测试，然后直接全量发布；指标异常时回滚。
```

显式调用：

```text
用 $chatgpt-pro-reviewer 评审下面的研究方案，重点判断核心创新、
最接近的已有工作，以及为什么顶会审稿人会关心：
……
```

也可以请求规划或第二意见：

```text
请让网页版 ChatGPT Pro 为这个迁移项目给出一个分阶段执行计划，
并指出最容易被忽略的依赖。
```

自动触发只表示 Codex 会在当前用户请求中匹配并加载 Skill。它不会在后台自行运行，也不会在没有发送授权时主动联系 ChatGPT。

### 工作流程

1. 根据当前请求形成一个简洁、边界明确的评审问题。
2. 使用用户明确指定的 ChatGPT 标签页；未指定时打开新的可见标签页。
3. 检查是否存在未发送草稿，避免覆盖用户内容。
4. 建立空白对话，并验证页面已登录且编辑框关联的模式明确显示为 `Pro`。
5. 写入完整提示词，再从编辑框读回核对。
6. 点击发送一次，并将结果绑定到本次创建的用户消息和对话 URL。
7. 等待相邻的助手回复生成完毕并稳定，然后返回忠实转录。

### 安全边界

- 仅用于规划、批评、评审或范围明确的第二意见。
- 每次明确请求最多发送一个文本提示词。
- 不上传文件，不在 ChatGPT 内启用工具或浏览功能，不更改账号设置。
- 不发送凭据、Cookie、私钥或无关的隐私数据。
- 发现疑似密钥时停止，并要求用户提供脱敏版本。
- 不向已有对话发送内容，避免污染用户原有上下文。
- 发送结果不明确时不会再次点击，以防重复提交。
- CAPTCHA、登录失效、限流或网络错误会原样报告，不会切换账号或传输方式。
- ChatGPT 的回复会作为外部建议处理；重要事实仍应独立核验。

### 已知限制

- 依赖 ChatGPT 当前网页结构和无障碍信息，页面改版可能需要更新 Skill。
- 需要保持有效登录状态，并且账号本身拥有 Pro 权限。
- CAPTCHA、限流和部分登录流程可能需要用户手动处理。
- 目前是一次性咨询工作流，不是后台任务、批处理器或聊天同步工具。
- 仅支持文本提示，不支持附件和多轮代理式交互。
- 网页显示 `Pro` 只证明发送时选择了该模式，不保证回复中的每项事实都正确。

### 项目结构

```text
.
├── SKILL.md
├── agents
│   └── openai.yaml
├── LICENSE
└── README.md
```

- `SKILL.md`：触发条件、网页操作流程和安全规则
- `agents/openai.yaml`：Codex 中的展示信息、默认提示词和隐式调用配置

### 许可证

本项目采用 [MIT License](LICENSE)。

---

## English

### Overview

A Codex Skill for calling ChatGPT Pro through its web interface.

It can:

- activate implicitly or through an explicit Skill mention;
- start a blank ChatGPT conversation;
- verify that the composer-associated setting displays `Pro`;
- enter and read back the complete prompt;
- click Send exactly once; and
- wait for a stable response before returning it to Codex.

It uses your existing browser login. No API key or private endpoint is required.

### Acceptance test

A real browser acceptance test completed successfully:

```text
CODEX_PRO_SESSION_OK

A launch plan should define rollback conditions for system instability,
degraded core business metrics, and data-security or compliance risks.
```

The test verified a separate conversation, visible `Pro` mode at the composer, a single submission, and a response matching the test question.

### Prerequisites

- Codex Desktop with visible-browser computer-use support
- An active ChatGPT web login
- Access to the `Pro` intelligence setting on that account
- Network access to `https://chatgpt.com/`

This Skill does not bypass subscription, authentication, regional, or account restrictions.

### Installation

Clone or download this repository, then run the following from its root:

```bash
skill_dir="${CODEX_HOME:-$HOME/.codex}/skills/chatgpt-pro-reviewer"
mkdir -p "$skill_dir"
cp SKILL.md "$skill_dir/"
cp -R agents "$skill_dir/"
```

If Codex does not detect it immediately, start a new Codex task or restart Codex.

### Usage

Implicit invocation:

```text
Ask webpage ChatGPT Pro to identify the largest risk in this launch plan:
run unit tests, deploy globally, and roll back if metrics look abnormal.
```

Explicit invocation:

```text
Use $chatgpt-pro-reviewer to critique this research proposal. Focus on its
core novelty, closest prior work, and why top-tier reviewers should care:
...
```

Planning and second-opinion requests are also supported:

```text
Ask webpage ChatGPT Pro for a phased migration plan and identify the
dependency most likely to be overlooked.
```

Implicit activation means Codex may match and load the Skill during the current user request. It does not run autonomously in the background or contact ChatGPT without authorization to send.

### How it works

1. Builds a concise, bounded review question from the current request.
2. Uses an explicitly mentioned ChatGPT tab, or opens a new visible tab.
3. Checks for existing drafts so user content is never overwritten.
4. Establishes a blank conversation and verifies login and composer-scoped `Pro` mode.
5. Writes the complete prompt and reads it back for verification.
6. Clicks Send once and anchors ownership to the resulting user turn and conversation URL.
7. Waits until the adjacent assistant response is complete and stable, then returns a faithful transcription.

### Safety boundaries

- Intended only for bounded planning, critique, review, and second opinions.
- Sends at most one text prompt for each explicitly authorized request.
- Does not upload files, enable ChatGPT tools or browsing, or modify account settings.
- Never sends credentials, cookies, private keys, or unrelated private data.
- Stops and requests redacted input when apparent secrets are present.
- Never sends into an existing conversation.
- Never retries Send after an uncertain click outcome.
- Reports CAPTCHA, login, rate-limit, access, and network blockers without switching accounts or transports.
- Treats the response as external advice; consequential factual claims still require independent verification.

### Limitations

- The workflow depends on ChatGPT's current UI and accessibility structure.
- A valid login and account-level Pro access are required.
- CAPTCHA, rate limits, and some authentication flows may require manual intervention.
- This is a one-shot consultation workflow, not a background agent, batch processor, or conversation-sync tool.
- Only text prompts are supported; attachments and multi-turn autonomous interaction are out of scope.
- Verifying the visible `Pro` setting does not guarantee that every returned claim is correct.

### Project structure

```text
.
├── SKILL.md
├── agents
│   └── openai.yaml
├── LICENSE
└── README.md
```

- `SKILL.md`: activation criteria, browser workflow, and safety rules
- `agents/openai.yaml`: Codex UI metadata, default prompt, and implicit-invocation configuration

### License

Licensed under the [MIT License](LICENSE).
