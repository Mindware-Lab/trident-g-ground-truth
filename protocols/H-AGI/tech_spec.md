 

**Recommended Architecture**
```text
ChatGPT / other LLM
        ↓
H-AGI MCP adapter
        ↓
H-AGI policy engine
├── readiness router
├── session state machine
├── metaprompt compiler
├── mode prompts
├── foil generator
├── human approval gates
├── causal-probe tracker
├── process scoring
└── delayed re-check scheduler
        ↓
Database / event log
```

The eight modes become versioned prompt modules:

```text
Comprehend | Challenge | Probe | Decide
Plan | Reframe | Create | Learn
```

Each generated metaprompt would combine:

```text
protocol invariants
+ readiness route
+ selected thinking mode
+ human-first input
+ current session state
+ foil requirements
+ output JSON schema
+ human-control constraints
```

**Important Separation**

Code should control:

- Workflow order and readiness routing
- State, permissions and confirmation gates
- Required human-first contribution
- Structured schemas and validation
- Scoring and delayed re-checks
- Audit and evaluation data

The LLM should generate:

- Models, alternatives and reframes
- Counterexamples and near-miss foils
- Candidate causal probes
- Explanations and structured suggestions

The human should retain:

- Initial framing
- Accept/reject/revise decisions
- Selection of real-world tests
- Final action and interpretation

This prevents the protocol from degrading into one elaborate prompt.

**ChatGPT Integration**

OpenAI’s current equivalent of a “plugin” is a **ChatGPT app using Apps SDK and MCP**. The MCP server defines typed tools; an optional widget can render assumption boards, decision matrices and causal-test boards directly inside ChatGPT. 

Possible tools:

```text
hagi.start_session
hagi.submit_human_first
hagi.run_mode
hagi.generate_foil
hagi.design_probe
hagi.record_judgment
hagi.record_outcome
hagi.schedule_recheck
hagi.get_profile
```

OpenAI recommends narrowly scoped tools with explicit input and output schemas, which fits this decomposition well. 

**One Limitation**

A ChatGPT app cannot guarantee that ChatGPT invokes every protocol step because the model chooses tools from their metadata. For strict H-AGI enforcement, use your own interface backed by the Responses API, while offering MCP as the ChatGPT adapter. Durable protocol state can live in your backend; OpenAI also supports stateful Responses/Conversations for model context. 

Given this repository already contains TypeScript/Vite applications, a natural location would be:

```text
apps/h-agi-coach/
├── server/          # MCP server and policy engine
├── web/             # optional ChatGPT widget
├── prompts/         # versioned mode definitions
├── schemas/         # session/tool/output contracts
└── tests/           # routing, foil and transfer evaluations
```

Security should include server-side input validation, explicit confirmation for consequential actions, minimal data retention, redacted logs and prompt-injection testing. 

---
The best UX would be a **hybrid of normal conversation and small interactive H-AGI panels rendered inside ChatGPT**. It should feel like a thinking coach, not a separate dashboard.

**User Journey**

1. The user enables **H-AGI Coach** from ChatGPT’s `+ → More` menu.
2. They write normally:

> Help me decide whether to launch this product next month.

3. ChatGPT invokes `hagi.start_session`.
4. An inline panel appears:

```text
H-AGI Session

Focus: Product launch decision
Session: Quick | Standard | Deep

Readiness:
Energy       1 2 3 4 5
Focus        1 2 3 4 5
Time pressure  Low / Medium / High
```

The backend converts this into a route such as `regulated`, `overloaded`, or `fast_brittle`. These should be described as session conditions, not psychological diagnoses.

**Human-First Gate**

Before ChatGPT supplies an answer, the user sees:

```text
Before asking AI

What do you currently think?
[________________________________]

What assumptions are you making?
[________________________________]

What outcome matters?
[________________________________]

[Continue]
```

This is the protocol’s most important UX feature. The AI answer remains gated until the user contributes some thinking.

**Thinking Mode**

The system then recommends a mode:

```text
Recommended: Decide

[Comprehend] [Challenge] [Probe] [Decide]
[Plan] [Reframe] [Create] [Learn]
```

The user can accept the recommendation or choose another mode.

ChatGPT calls something like:

```text
hagi.run_mode(
  session_id,
  mode="decide",
  human_first_input=...
)
```

**Working Board**

ChatGPT responds conversationally, accompanied by an inline structured board:

```text
Decision Board

Options
1. Launch next month
2. Delay launch
3. Limited pilot

Critical assumptions
- Demand is sufficiently validated
- Support capacity is adequate
- Delay has a meaningful cost

Main uncertainty
Whether current interest converts to paid demand
```

The user can edit the board or continue through chat:

> Challenge the demand assumption.

The chatbot then calls `hagi.generate_foil`, updates the board and explains the result.

**Challenge And Probe**

A challenge stage might show:

```text
Possible failure

Current evidence measures expressed interest,
not willingness to pay.

What would weaken your current position?
[________________________________]

Suggested smallest safe test:
Offer a paid pilot to 20 qualified prospects.

[Use this probe] [Modify] [Generate another]
```

The chatbot generates candidates, but the user selects the actual test.

**Human Judgement Gate**

Before completing the session:

```text
Your judgement

[Accept] [Revise] [Test first] [Delay decision]

Confidence:  20% ─────●───── 100%

Reason:
[________________________________]
```

This calls `hagi.record_judgment`; the model does not silently decide on the user’s behalf.

**Session Output**

The user receives a concise decision record:

```text
Decision: Run a limited paid pilot
Main assumption: Interest converts to payment
Probe: Offer pilot to 20 prospects
Success threshold: 4 paid commitments
Review date: 20 June 2026
Current confidence: 65%
```

Later, they can say:

> Continue my launch session.

The app retrieves the session and asks what actually happened before suggesting an update.

**Chatbot Interface**

Under the surface:

```text
User message
    ↓
ChatGPT decides which H-AGI tool to invoke
    ↓
MCP server validates the workflow
    ↓
Tool returns structured JSON
    ↓
ChatGPT explains it conversationally
    +
Inline widget renders controls and boards
```

The widget can call MCP tools directly when buttons are pressed and update the context visible to ChatGPT. OpenAI supports these components as iframes rendered inline in the conversation.

**Best MVP UX**

For a same-day build, I would implement:

- Mostly conversational interaction
- One compact session panel
- Human-first gate
- Mode selection
- Structured model/challenge/probe output
- Final judgement buttons
- Persistent session ID

Skip dashboards, accounts and complex visual boards initially. The user should be able to start with:

> Use H-AGI to help me think through this.

A full session should then take 5–15 minutes without requiring the user to understand metaprompts, tools or MCP.

One limitation: a ChatGPT app operates inside inline components and tool calls; it cannot replace ChatGPT’s whole interface. For absolutely strict workflow enforcement, a dedicated H-AGI web application using the OpenAI API would provide more control.

Sources: [Connect from ChatGPT](https://developers.openai.com/apps-sdk/deploy/connect-chatgpt), [Build ChatGPT UI](https://developers.openai.com/apps-sdk/build/chatgpt-ui), [Define tools](https://developers.openai.com/apps-sdk/plan/tools).

So: **yes, this is technically feasible, and MCP plus a deterministic policy engine is substantially better suited than a custom metaprompt alone.**

Sources: [Apps SDK quickstart](https://developers.openai.com/apps-sdk/quickstart), [MCP concepts](https://developers.openai.com/apps-sdk/concepts/mcp-server), [tool design](https://developers.openai.com/apps-sdk/plan/tools), [state management](https://developers.openai.com/apps-sdk/build/state-management).
