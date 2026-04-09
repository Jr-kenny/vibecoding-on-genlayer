# GenLayer Builder Guide and Project Portfolio

This repository is my GenLayer learning path, project portfolio, and practical builder guide for developers who want to understand Intelligent Contracts well enough to build with them, test them, and connect them to real frontends.

The goal is not just to explain GenLayer in simple words. The goal is to show that I understand the protocol at the level an actual builder needs:

- What GenLayer adds beyond ordinary smart contracts.
- How Intelligent Contracts use Python, web access, and LLMs safely.
- How the GenVM handles non-deterministic execution.
- How Optimistic Democracy and the Equivalence Principle make AI and web data usable in a blockchain setting.
- How to choose between strict equality, custom validator functions, comparative validation, and non-comparative validation.
- How to test, debug, deploy, and connect a GenLayer contract to a frontend without hallucinating contract functions.

## How to Use This Guide

This README is written for two groups at the same time:

- New vibecoders who have never touched GenLayer and need a clear path from "I do not know what to do" to "I can build and submit something."
- Experienced vibecoders or coders who already know how to ship apps and need the GenLayer-specific rules that stop AI-generated projects from breaking.

If you are new, do not start by memorizing every protocol term. Start with the beginner path below, then come back to the deeper sections when you need them.

If you already code, skim the beginner path, then focus on these sections:

- [The Equivalence Principle](#the-equivalence-principle)
- [Non-Determinism Rules Builders Must Respect](#non-determinism-rules-builders-must-respect)
- [Development Workflow That Actually Works](#development-workflow-that-actually-works)
- [Frontend Integration with GenLayerJS](#frontend-integration-with-genlayerjs)
- [Debugging Checklist](#debugging-checklist)

## New Vibecoder Start Here

If you have no idea what to do, follow this path in order.

### Step 1: Understand the One-Sentence Version

GenLayer lets you build smart contracts that can use AI and web data, while validators still agree on whether the result is acceptable.

That is the whole point.

Normal smart contract:

```text
Input -> deterministic code -> exact same output on every node
```

GenLayer Intelligent Contract:

```text
Input -> AI/web-aware Python contract -> leader result -> validators check if the result is acceptable
```

### Step 2: Learn the 7 Words That Matter

| Word | Simple Meaning |
| --- | --- |
| Intelligent Contract | A Python smart contract that can use AI, web data, and normal contract state. |
| GenVM | The runtime that executes Intelligent Contracts. |
| Validator | A node that checks whether a transaction result should be accepted. |
| Leader | The validator that proposes the first result for a transaction. |
| Equivalence Principle | The rule for deciding whether validator outputs are close enough or correct enough. |
| `view` method | A read-only function. It does not change state and is fast. |
| `write` method | A state-changing function. It costs gas and needs a transaction receipt. |

If you understand only these words, you can start building.

### Step 3: Pick a Beginner-Friendly Project

Do not start with a huge marketplace or full AI agent. Start with a project where the input, output, and validation rule are easy to explain.

Good beginner ideas:

- AI word generator: user asks for a word, contract stores the generated word and definition.
- Article summarizer: contract fetches a URL and stores a short summary.
- Sentiment checker: contract classifies text as positive, neutral, or negative.
- Price tracker: contract fetches a price and accepts small differences between validators.
- Event result checker: contract reads a source and stores a structured result like winner, score, or status.

Avoid these at first:

- Multi-contract systems.
- Payments plus AI plus marketplace logic all in one project.
- Anything where you cannot explain the validator rule in one sentence.
- Any frontend that guesses contract method names.

### Step 4: Choose the Right Equivalence Rule

This choice is where many AI-generated GenLayer projects fail.

Use this simple decision table:

| Your project needs | Use this pattern |
| --- | --- |
| Exact answer from a stable source | Strict equality |
| Number that can move a little, like a price | Custom validator with tolerance |
| JSON result with important fields and optional explanation | Partial field matching |
| Two text answers that should mean the same thing | Comparative validation |
| A creative or summary output where many answers can be valid | Non-comparative validation |

Examples:

- Price is `100.00` vs `100.50`: custom validator with tolerance.
- Winner is `Team A` but analysis text is different: partial field matching.
- Summary is different wording but still faithful: non-comparative validation.
- Raw LLM paragraph must match exactly: usually a bad idea.

### Step 5: Build in the Right Order

Use this order even if you are using an AI builder:

1. Write or generate the Intelligent Contract first.
2. Identify every public `view` and `write` method.
3. Decide the equivalence rule for every AI or web call.
4. Test the contract logic.
5. Deploy or run it in Studio.
6. Copy the real contract address and real method names.
7. Build the frontend using those exact method names.
8. Wait for transaction receipts after every write.
9. Record your evidence: repo links, screenshots, contract address, network, and what the project proves.

Do not ask an AI frontend builder to "connect my GenLayer contract" until you can give it:

- The contract address.
- The target network.
- The exact `view` method names.
- The exact `write` method names.
- The arguments each method needs.
- The expected return shape.

If you do not know your method names, arguments, or return shapes, paste your contract code into ChatGPT, Claude, Gemini, or another AI model and ask:

```text
Read this GenLayer Intelligent Contract and extract the frontend handoff details.

Return:
- Contract class name.
- Constructor arguments.
- All @gl.public.view methods with method name, arguments, and return type.
- All @gl.public.write methods with method name, arguments, and return type.
- Any method that appears to return JSON or a structured object.
- The expected return shape for each method.
- A short frontend handoff summary I can paste into Lovable or another frontend builder.

Contract code:
[PASTE CONTRACT CODE HERE]
```

### Step 6: Use This Minimal Build Checklist

Before submitting, your project should answer these questions:

| Question | Your Answer Should Be Clear |
| --- | --- |
| What does the project do? | One sentence, no buzzwords. |
| Why does it need GenLayer? | It uses AI, web data, or non-deterministic logic that normal smart contracts do not handle well. |
| What is the Intelligent Contract? | Link the contract file or repo. |
| Which method reads state? | Name the `@gl.public.view` methods. |
| Which method changes state? | Name the `@gl.public.write` methods. |
| What equivalence rule is used? | Strict, custom tolerance, partial field matching, comparative, or non-comparative. |
| How did you test it? | Direct test, Studio test, or manual Studio proof. |
| How does the frontend connect? | GenLayerJS read/write calls using real method names. |
| What happens after a write? | The UI waits for `ACCEPTED` or `FINALIZED`. |

## Quick Paths by Skill Level

### Path A: New Vibecoder

Use this path if you mostly build through AI tools.

1. Read [New Vibecoder Start Here](#new-vibecoder-start-here).
2. Open [GenLayer Studio](https://studio.genlayer.com).
3. Build a small contract with one `view` method and one `write` method.
4. Use the [A Strong AI-Builder Prompt for GenLayer Frontends](#a-strong-ai-builder-prompt-for-genlayer-frontends).
5. Do not let the AI guess method names.
6. Test one full flow: read -> write -> wait for receipt -> read again.
7. Submit the repo with the contract, frontend, screenshots, and a short explanation of your equivalence rule.

### Path B: Experienced Vibecoder

Use this path if you can already ship apps but are new to GenLayer.

1. Use the [GenLayer Project Boilerplate](https://github.com/genlayerlabs/genlayer-project-boilerplate).
2. Put contracts in `contracts/`.
3. Run `genvm-lint check`.
4. Add direct tests with mocked web or LLM responses.
5. Add a small Studio or network integration test.
6. Build typed GenLayerJS wrapper functions for the frontend.
7. Add transaction states in the UI: idle, submitted, accepted, finalized, failed.
8. Document the equivalence rule beside the contract or in the README.

### Path C: Established Coder

Use this path if you already understand smart contracts and want the GenLayer-specific depth.

1. Design the non-deterministic boundary first.
2. Keep `gl.nondet.*` inside leader or validator functions.
3. Keep storage writes outside non-deterministic blocks.
4. Prefer custom validators when the business logic needs exact control.
5. Compare stable fields, not raw LLM prose.
6. Treat `UNDETERMINED` as a valid safety outcome for ambiguous inputs.
7. Test validator agreement and disagreement, not just happy paths.
8. Use `FINALIZED` only when the UX or risk level requires finality-window completion.

## Beginner Project Recipes

Use one of these recipes if you do not know what to build yet. Each recipe is intentionally small, because a small complete GenLayer project scores better than a large broken one.

### Recipe 1: AI Word Generator

Best for a first GenLayer project.

What it does:

- User clicks a button.
- Contract asks an LLM for a word and definition.
- Contract stores the word and definition.
- Frontend reads the stored result.

Suggested contract methods:

| Method | Type | Purpose |
| --- | --- | --- |
| `get_word()` | `view` | Returns the current stored word data. |
| `generate_word(topic: str)` | `write` | Uses AI to generate a new word for a topic and stores it. |

Suggested equivalence rule:

- Use partial field matching if the contract returns structured JSON like `word`, `definition`, and `category`.
- Compare the important fields, not long free-form reasoning.

Why this is good for beginners:

- It proves AI usage.
- It has one clear read and one clear write.
- The frontend is simple.
- The validation rule is easy to explain.

### Recipe 2: Article Summarizer

Best for learning web access plus AI.

What it does:

- User provides a URL.
- Contract fetches the article or page content.
- Contract asks an LLM for a short summary.
- Contract stores the summary.

Suggested contract methods:

| Method | Type | Purpose |
| --- | --- | --- |
| `get_summary(url: str)` | `view` | Returns the stored summary for a URL. |
| `summarize_url(url: str)` | `write` | Fetches content, summarizes it, and stores the result. |

Suggested equivalence rule:

- Use non-comparative validation if many different summaries can be valid.
- The validator should check that the summary is faithful, short, and does not invent facts.

Why this is good for serious builders:

- It forces you to think about source reliability.
- It demonstrates why exact string matching is weak for summaries.
- It gives a strong reason for GenLayer: web data plus LLM reasoning.

### Recipe 3: Price or Data Tracker

Best for learning custom validators.

What it does:

- Contract fetches a price or numeric value from an API.
- Validators may see slightly different values because timing differs.
- Contract accepts the result only if the validator's value is within a defined tolerance.

Suggested contract methods:

| Method | Type | Purpose |
| --- | --- | --- |
| `get_price(pair: str)` | `view` | Returns the latest stored price for a pair. |
| `update_price(pair: str)` | `write` | Fetches and stores the price if validators agree within tolerance. |

Suggested equivalence rule:

- Use a custom validator with a percentage tolerance, such as `<= 2%`.
- If the leader returns `0`, handle it as a special case to avoid division errors.

Why this is good for established coders:

- It shows real consensus design.
- It is easy to test with mocked API responses.
- It proves you understand that validators may not always fetch the exact same value.

## AI Prompt Helper for Newbies

This section is for people who do not have Codex, Claude Code, Cursor, or a local coding setup. You can use it in normal chat apps like ChatGPT, Claude web, Gemini, or any other AI model.

The flow is:

```text
Prompt AI for contract -> paste contract into GenLayer Studio -> fix errors if Studio shows any -> deploy -> copy contract address and methods -> use Lovable or another frontend builder
```

### Universal Contract Generator Prompt

Copy this into ChatGPT, Claude, Gemini, or another AI model.

```text
You are an expert GenLayer Intelligent Contract developer and a patient teacher for a beginner.

I want you to help me generate a GenLayer Intelligent Contract that I can paste into GenLayer Studio and deploy.

Important context:
- GenLayer Intelligent Contracts are written in Python.
- The contract class must extend gl.Contract.
- Public read methods must use @gl.public.view.
- Public write methods must use @gl.public.write.
- The contract should use type annotations so GenLayer Studio can detect constructor inputs and method schemas.
- All gl.nondet.* calls, including LLM prompts and web requests, must be inside a nondeterministic function/block.
- Do not write to contract storage inside leader_fn, validator_fn, or any nondeterministic block.
- Storage writes should happen only after the consensus result has been returned.
- Do not guess advanced imports. Keep the contract beginner-friendly.

My project idea:
[PASTE YOUR IDEA HERE IN ONE OR TWO SENTENCES]

Before writing code, ask me these questions:
1. What should the contract do in one sentence?
2. Should it use an LLM, web data, or both?
3. What information should be stored on-chain?
4. What should users be able to read from the contract?
5. What should users be able to change or generate with a transaction?
6. What should the AI or web result look like? If JSON is needed, define the exact fields.
7. Which equivalence rule fits best: strict equality, custom tolerance, partial field matching, comparative validation, or non-comparative validation?

After I answer, generate:
1. The complete GenLayer Python contract code in one code block.
2. A simple explanation of every method.
3. A table with method name, method type, arguments, return type, and frontend usage.
4. The constructor arguments I should enter in GenLayer Studio, if any.
5. The equivalence rule used and why.
6. A Studio deployment checklist.
7. A frontend handoff summary I can paste into Lovable or another frontend builder.

Rules for the contract:
- Keep it small: one contract, simple state, clear method names.
- Include at least one @gl.public.view method.
- Include at least one @gl.public.write method if the project changes state.
- Use safe parsing and clear error messages when working with JSON.
- If using an LLM, ask for structured JSON when possible.
- If using web data, fetch only from a stable and trusted source.
- If comparing numeric data, use a custom validator with tolerance.
- If comparing structured AI results, compare the stable fields instead of long free-form explanation.
- If summarizing or creating open-ended text, explain why non-comparative validation may be better.

Do not:
- Do not generate frontend code yet.
- Do not invent a deployed contract address.
- Do not say the code is deployed until I deploy it myself.
- Do not use raw LLM paragraph output with strict equality unless the output is intentionally constrained.
- Do not omit method type annotations.
- Do not hide important method names from the final summary.
```

### Fast Version for People Who Hate Long Prompts

Use this shorter version if the AI model gets overwhelmed:

```text
Create a beginner-friendly GenLayer Intelligent Contract in Python for this idea: [PASTE IDEA].

Requirements:
- Use from genlayer import *.
- Create one class extending gl.Contract.
- Add clear typed state variables.
- Add at least one @gl.public.view method.
- Add at least one @gl.public.write method if state changes.
- Keep all gl.nondet.* calls inside nondeterministic functions.
- Do not write to storage inside nondeterministic blocks.
- Use an appropriate equivalence rule and explain it.
- Include type annotations so GenLayer Studio can detect the schema.
- Return a method table for frontend builders: method name, view/write, args, return type, and purpose.
- Include Studio deployment steps and likely errors.

Do not generate frontend code yet. I only need the contract and the frontend handoff summary.
```

### Prompt to Fix Studio Errors

If GenLayer Studio shows an error after you paste the contract, copy the error and use this prompt:

```text
I pasted this GenLayer Intelligent Contract into GenLayer Studio and got an error.

Contract code:
[PASTE CONTRACT CODE]

Studio error or log:
[PASTE THE EXACT ERROR OR SCREEN TEXT]

Please fix the contract for GenLayer Studio.

Check specifically for:
- Python syntax errors.
- Missing from genlayer import *.
- Missing type annotations.
- Wrong @gl.public.view or @gl.public.write usage.
- Constructor parameters without clear types.
- gl.nondet.* calls outside nondeterministic functions.
- Storage writes inside nondeterministic functions.
- Invalid JSON parsing.
- Method names that are unclear for frontend use.

Return:
1. The corrected full contract code.
2. What changed.
3. What I should try next in GenLayer Studio.
4. A method table for the frontend.
```

### If Studio Cannot Load the Contract Schema

GenLayer Studio detects constructor inputs and contract methods from your code. If it cannot load the contract schema, it usually means the code is not easy for the schema detector to understand.

Try these fixes:

- Make sure the contract has `from genlayer import *`.
- Make sure there is exactly one main contract class extending `gl.Contract`.
- Add clear type annotations to constructor arguments, for example `def __init__(self, topic: str):`.
- Add return types to public methods, for example `def get_summary(self) -> str:`.
- Use `@gl.public.view` for read methods.
- Use `@gl.public.write` for state-changing methods.
- Avoid clever dynamic method creation.
- Avoid huge multi-class contracts for your first project.
- Remove unused complex imports if the AI added them.
- If the constructor has too many parameters, simplify it and set defaults inside the contract.
- Paste the exact schema/loading error back into the AI model using the repair prompt above.

The technical reason this matters: GenLayer exposes contract interface information through schema detection. That schema includes constructor inputs and method information such as method names, parameters, whether a method is readonly, whether it is payable, and the return type. Your frontend builder needs that method information later.

Official reference: [gen_getContractSchema](https://docs.genlayer.com/api-references/genlayer-node/gen/gen_getContractSchema).

### Studio Deployment Checklist for Newbies

After the AI gives you contract code:

1. Open [GenLayer Studio](https://studio.genlayer.com).
2. Create or open a contract file.
3. Paste the generated Intelligent Contract code.
4. Check whether Studio detects constructor inputs.
5. If constructor inputs appear, fill them in. Use JSON mode if you need to enter structured values.
6. Click Deploy.
7. Copy the deployed contract address.
8. Open the read methods and call the simplest read method first.
9. Run one write method.
10. Wait for the transaction to be accepted.
11. Call the read method again to confirm state changed.
12. Save the contract address, network, method names, argument list, and return shapes.

Official reference: [Deploy Contracts](https://docs.genlayer.com/developers/intelligent-contracts/tools/genlayer-studio/deploying-contract).

### Frontend Handoff Block for Lovable or Other Builders

After deployment, fill this out and paste it into Lovable, v0, Bolt, ChatGPT, Claude, Gemini, or any frontend builder.

```text
Build a frontend for my deployed GenLayer Intelligent Contract.

Network:
[localnet, studionet, testnetAsimov, or testnetBradbury]

Contract address:
[PASTE CONTRACT ADDRESS]

Contract purpose:
[ONE SENTENCE]

Read methods:
1. Method name: [name]
   Args: [args]
   Return type/shape: [return]
   UI use: [what this displays]

Write methods:
1. Method name: [name]
   Args: [args]
   Return type/shape: [return]
   UI use: [what button/form triggers this]

Transaction rule:
- For every write method, send the transaction, show the transaction hash, then wait for waitForTransactionReceipt.
- Use ACCEPTED for normal UI confirmation unless I say FINALIZED.
- Do not show success just because a transaction hash exists.

Error handling:
- Show a clear error if the contract address is missing.
- Show a clear error if the wallet/network is wrong.
- Show a clear error if the method name is not found.
- If returned data is JSON, parse it with try/catch.
- If data is malformed, show a fallback message instead of a blank screen.

Do not:
- Do not invent contract methods.
- Do not rename my methods.
- Do not assume the contract returns word/definition unless I listed those fields.
- Do not hardcode private keys in the frontend.
- Do not claim the transaction succeeded until the receipt reaches ACCEPTED or FINALIZED.
```

After the contract is ready, use the stronger frontend prompt later in this README if you want a more detailed UI build.

## Project Evidence

These are the resources and example projects I created or used while learning and building on GenLayer:

| Resource | Purpose |
| --- | --- |
| [Creating an Intelligent Contract on GenLayer](https://github.com/Jr-kenny/INTELLIGENT-CONTRACT-WITH-GENLAYER-STUDIO-) | A practical starting point for creating and interacting with an Intelligent Contract in GenLayer Studio. |
| [All I Know About GenLayer](https://github.com/Jr-kenny/What-i-know-about-genlayer) | My broader notes and explanation of the protocol. |
| [GenLayer Events](https://github.com/Jr-kenny/Genlayer-Events) | Example project exploring event-style GenLayer use cases. |
| [Prime Valuator](https://github.com/Jr-kenny/Prime-valuator) | Example project for evaluation-style logic. |
| [Prime Mall Customer Service](https://github.com/Jr-kenny/Prime-mall-customer-service) | Example AI/customer-service style experiment. |
| [Wordwise](https://github.com/Jr-kenny/Wordwise) | Example project for AI-generated or language-focused output. |
| [Official GenLayer Docs](https://docs.genlayer.com/) | Primary documentation source. |
| [GenLayer Project Boilerplate](https://github.com/genlayerlabs/genlayer-project-boilerplate) | Recommended production-style starter with contracts, tests, deploy scripts, and frontend structure. |

## Example Apps From My GitHub Profile

I searched my public GitHub profile and checked the likely GenLayer app READMEs for GenLayer, Intelligent Contract, Studionet, or contract-address evidence. These examples give new builders real project directions they can study instead of starting from a blank page.

| Example | What It Demonstrates | What New Builders Can Learn |
| --- | --- | --- |
| [Wagerverse](https://github.com/Jr-kenny/Wagerverse) | A game wagering app where payouts happen after GenLayer judging plus relayer settlement. | How GenLayer can act as a judgment layer while another chain handles wager/payment settlement. |
| [Primequiz](https://github.com/Jr-kenny/Primequiz) | An AI-powered quiz platform on GenLayer using Intelligent Contracts. | How education apps can generate or verify questions dynamically instead of using only static quizzes. |
| [Prime Valuator](https://github.com/Jr-kenny/Prime-valuator) | An AI evaluation contract for making decisions about complex information. | How to turn messy input into a score, verdict, or decision that can be stored and shown to users. |
| [Prime Mall Customer Service](https://github.com/Jr-kenny/Prime-mall-customer-service) | An Intelligent Contract concept for customer care and escalation. | How support apps can combine routine answers with AI judgment for harder cases. |
| [Wordwise](https://github.com/Jr-kenny/Wordwise) | An AI-powered word guessing game on GenLayer. | How to use AI-generated game content while still keeping gameplay fair and inspectable. |
| [VerdictDotFun](https://github.com/Jr-kenny/verdictdotfun) | A GenLayer multiplayer game app with on-chain profiles, room matchmaking, and contract-native match resolution. | How GenLayer can be used for game state, identity, leaderboards, and match verdicts. |
| [ShipSignal](https://github.com/Jr-kenny/shipsignal) | A shipping claim review tool with a GenLayer Studionet contract. | How operational support teams can turn timelines and claims into reasoned decisions. |
| [ProofMatch](https://github.com/Jr-kenny/proofmatch) | A hiring fit evaluator that turns candidate profiles into onchain shortlist decisions. | How to build structured AI scoring for HR or community hiring workflows. |
| [PrizePilot](https://github.com/Jr-kenny/prizepilot) | A hackathon or grant submission scoring engine. | How to score projects against a rubric and return a verdict, score, and explanation. |
| [PolicyGate](https://github.com/Jr-kenny/policygate) | A compliance and messaging checker for copy, policies, and launch reviews. | How to build policy-based AI gates where the output is safe, needs edits, or blocked. |
| [GrantJudge](https://github.com/Jr-kenny/grantjudge) | A grant application screener for ecosystem funds and accelerators. | How to compare an application against a fund thesis and produce a ranked decision. |
| [GovPulse](https://github.com/Jr-kenny/govpulse) | A governance proposal triage tool for DAOs and treasury stewards. | How GenLayer can compress long proposals into pass, review, or block decisions with reasons. |
| [DisputeDock](https://github.com/Jr-kenny/disputedock) | An evidence-based dispute resolution tool for marketplaces and freelance platforms. | How to turn agreements and evidence into an onchain payout recommendation. |
| [DealGuard](https://github.com/Jr-kenny/dealguard) | A pricing exception gate for sales and finance teams. | How to use AI judgment against a policy to approve, review, or block a discount. |
| [GenLayer MCP](https://github.com/Jr-kenny/Genlayer_mcp) | A docs-only MCP server that exposes GenLayer documentation to AI coding tools. | How documentation tooling can make builders more accurate when working with GenLayer. |
| [Intelligent Contract With GenLayer Studio](https://github.com/Jr-kenny/INTELLIGENT-CONTRACT-WITH-GENLAYER-STUDIO-) | A beginner guide for deploying Intelligent Contracts with GenLayer Studio. | How new builders can move from contract code to Studio deployment without local tooling. |
| [GenLayer Events](https://github.com/Jr-kenny/Genlayer-Events) | A webpage for GenLayer community activities and events. | How ecosystem/community pages can support builders outside pure contract code. |

Flagship example: [VerdictDotFun](https://github.com/Jr-kenny/verdictdotfun) is the strongest portfolio piece here because it goes beyond a single-form evaluator. It combines a GenLayer game app, persistent on-chain player profiles, live room-based matchmaking, contract-native multiplayer gameplay, identity, leaderboard state, room registry, and match verdict logic. For new builders, it is the advanced example to study after they understand smaller apps like Wordwise or PrizePilot.

The pattern across these projects is important: each app has a clear real-world decision or interaction that benefits from GenLayer because the result is not just a normal deterministic calculation. New builders can copy the shape of the idea, but they should still write their own contract methods, equivalence rule, and frontend handoff.

## Why GenLayer Matters

Traditional blockchains are strongest when every node can calculate the exact same result from the exact same input. That works well for token balances, deterministic game rules, and simple contract logic. It breaks down when a contract needs to reason about messy real-world inputs:

- Web pages that change over time.
- APIs that return slightly different data depending on timing.
- LLM responses that are semantically correct but not byte-for-byte identical.
- Human-language decisions where the correct result depends on context.

GenLayer extends the smart contract model with Intelligent Contracts. Intelligent Contracts are Python contracts that can use natural language processing, web connectivity, and non-deterministic operations while still preserving blockchain-style consensus.

The important part is not "AI on-chain" as a slogan. The important part is that GenLayer gives developers a consensus mechanism for outputs that cannot always be reproduced exactly.

## The Core Mental Model

An Intelligent Contract is a smart contract that can reason over the outside world.

The contract still has state, public methods, transactions, gas, and deployment. The difference is that GenLayer lets the contract include controlled non-deterministic operations such as LLM calls and web requests.

That creates a new problem: if five validators ask an LLM to summarize the same article, they may get five different summaries. Traditional blockchains would treat that as a failure. GenLayer solves it by asking a more useful question:

Do the validators agree that the leader's result is acceptable under the contract's equivalence rules?

That one shift is the center of GenLayer.

## GenLayer Architecture in Builder Terms

### Intelligent Contracts

Intelligent Contracts are written in Python with the GenVM SDK. A basic contract includes:

- A class that extends `gl.Contract`.
- State variables declared with type annotations.
- Public read methods decorated with `@gl.public.view`.
- Public write methods decorated with `@gl.public.write`.
- Payable write methods decorated with `@gl.public.write.payable` when they receive value.

Minimal shape:

```python
# { "Depends": "py-genlayer:<version-or-hash>" }
from genlayer import *


class Storage(gl.Contract):
    value: str

    def __init__(self, initial_value: str):
        self.value = initial_value

    @gl.public.view
    def get_value(self) -> str:
        return self.value

    @gl.public.write
    def set_value(self, new_value: str):
        self.value = new_value
```

A strong submission should make this split clear:

- `view` methods are reads. They do not change state, do not require a transaction, and return immediately.
- `write` methods change state. They require a signed transaction, consume gas, and must be accepted or finalized by the network before the state change is trusted.
- Non-deterministic calls are not normal Python calls. They must be wrapped in GenLayer's consensus patterns.

Official reference: [Introduction to Intelligent Contracts](https://docs.genlayer.com/developers/intelligent-contracts/introduction).

### GenVM

The GenVM is the execution environment for Intelligent Contracts. Its purpose is to execute Python contracts that may include non-deterministic logic while maintaining network consistency.

The GenVM is different from a traditional blockchain VM because it supports:

- LLM integration.
- Internet access.
- Python-based contract development.
- Execution rules for non-deterministic code.

The practical builder takeaway is this: do not treat GenVM as just another EVM clone. It is designed for AI-powered and web-connected contract execution, so contract structure, validation, and testing must account for non-determinism from the start.

Official reference: [GenVM](https://docs.genlayer.com/understand-genlayer-protocol/core-concepts/genvm).

### Optimistic Democracy

GenLayer uses Optimistic Democracy as its consensus method for Intelligent Contract execution.

In plain builder terms:

1. A transaction is submitted to an Intelligent Contract.
2. A group of validators is selected.
3. One validator becomes the leader and proposes the result.
4. Other validators evaluate the leader's result using the contract's equivalence rule.
5. If a majority agrees, the transaction is accepted.
6. During the Finality Window, an incorrect result can be appealed.
7. Appeals bring in more validators and can escalate until the dispute is settled.

This matters because GenLayer does not pretend every AI or web result will be identical. Instead, it builds a validator process around whether the result is equivalent enough for the contract's declared purpose.

Official reference: [Optimistic Democracy](https://docs.genlayer.com/understand-genlayer-protocol/core-concepts/optimistic-democracy).

## The Equivalence Principle

The Equivalence Principle is the most important concept for building serious GenLayer applications.

It answers this question:

When a contract uses a web request, an LLM call, or another operation that may produce different outputs on different nodes, how should validators decide whether the leader's result should be accepted?

A weak GenLayer project says "AI decides." A strong GenLayer project defines the exact equivalence rule.

### Pattern 1: Strict Equality

Use strict equality when validators can reproduce the same normalized output.

Good use cases:

- Stable API result after normalization.
- JSON data where keys are sorted and irrelevant fields are removed.
- Blockchain RPC results.
- Objective values that should match exactly.

Example shape:

```python
def fetch_height():
    response = gl.nondet.web.request("https://api.example.com/block/latest")
    data = json.loads(response)
    return json.dumps(
        {"height": data["height"], "hash": data["hash"]},
        sort_keys=True,
    )


height = gl.eq_principle.strict_eq(fetch_height)
```

Do not use strict equality for raw LLM output unless the prompt forces an extremely constrained response and the validators can actually converge.

### Pattern 2: Custom Validator Function

For most serious projects, the best pattern is a custom leader and validator function with `gl.vm.run_nondet_unsafe`.

This gives the contract full control over:

- What the leader returns.
- How validators reproduce or verify the output.
- Which fields matter.
- Whether numeric tolerance is allowed.
- How errors are classified.
- Whether ambiguous results should become undetermined instead of being forced through.

Example: a price feed where values may drift slightly between the leader and validators:

```python
@gl.public.write
def update_price(self, pair: str):
    url = f"https://api.example.com/prices/{pair}"

    def leader_fn():
        response = gl.nondet.web.get(url)
        data = json.loads(response.body.decode("utf-8"))
        return float(data["price"])

    def validator_fn(leader_result) -> bool:
        if not isinstance(leader_result, gl.vm.Return):
            return False

        validator_price = leader_fn()
        leader_price = leader_result.calldata

        if leader_price == 0:
            return validator_price == 0

        return abs(leader_price - validator_price) / abs(leader_price) <= 0.02

    agreed_price = gl.vm.run_nondet_unsafe(leader_fn, validator_fn)
    self.prices[pair] = agreed_price
```

This is stronger than saying "compare the result" because the tolerance is explicit.

### Pattern 3: Partial Field Matching

Many AI tasks return both objective fields and subjective explanation.

Example:

```json
{
  "analysis": "The source says Team A won after extra time.",
  "winner": "Team A",
  "score": "2-1"
}
```

Two validators may produce different `analysis` text but agree on `winner` and `score`. In that case, the validator should compare the decision fields and avoid comparing free-form explanation text.

This pattern is useful for:

- Sports results.
- Event outcomes.
- Claim checks with structured verdicts.
- Support-ticket classification.
- AI scoring where the explanation can vary but the final label matters.

### Pattern 4: Comparative Validation

Comparative validation is useful when two outputs are not identical, but an LLM can judge whether they mean the same thing under the contract's principle.

Good use cases:

- "The outcome must match exactly, but reasoning can differ."
- "The extracted category must be the same."
- "The summary must contain the same key facts."
- "The sentiment label must be equivalent."

GenLayer provides convenience wrappers such as `prompt_comparative`, but a strong project should be willing to move into custom validators when the business rule becomes more specific.

### Pattern 5: Non-Comparative Validation

Non-comparative validation is useful when validators should not repeat the leader's work. Instead, validators judge whether the leader's output satisfies the task and criteria.

Good use cases:

- Summarization where many different summaries can all be valid.
- Creative or open-ended generation.
- Output quality checks where reproducing the exact task is wasteful or less useful.

Example idea:

```python
result = gl.eq_principle.prompt_non_comparative(
    lambda: gl.nondet.web.get(url).body.decode("utf-8"),
    task="Summarize this article in 2-3 sentences",
    criteria="""
    The summary must capture the main point.
    The summary must not add facts that are not in the source.
    The summary must be 2-3 sentences.
    """,
)
```

This should be used carefully. If the output can be reduced to objective fields, partial field matching or a custom validator is usually stronger.

Official reference: [The Equivalence Principle](https://docs.genlayer.com/developers/intelligent-contracts/equivalence-principle).

## Non-Determinism Rules Builders Must Respect

GenLayer's non-deterministic features are powerful, but they are not a free-for-all.

The most important rule:

All `gl.nondet.*` calls must happen inside a non-deterministic block.

That means web requests and LLM calls belong inside `leader_fn`, `validator_fn`, or a function passed to an equivalence-principle helper.

Do this:

```python
@gl.public.write
def fetch_price(self):
    def leader_fn():
        response = gl.nondet.web.get("https://api.example.com/price")
        return json.loads(response.body.decode("utf-8"))["price"]

    def validator_fn(leader_result) -> bool:
        if not isinstance(leader_result, gl.vm.Return):
            return False
        validator_price = leader_fn()
        return abs(leader_result.calldata - validator_price) <= 1

    self.price = gl.vm.run_nondet_unsafe(leader_fn, validator_fn)
```

Do not write contract state inside the non-deterministic block.

Bad pattern:

```python
def leader_fn():
    response = gl.nondet.web.get("https://api.example.com/price")
    self.price = response.body  # Wrong: side effect before consensus.
    return response.body
```

Why this matters:

- Validators execute non-deterministic blocks independently.
- Each validator may see different web timing, API state, or LLM output.
- Storage writes, cross-contract calls, and messages must happen after consensus agrees on the result.

Official reference: [Non-determinism](https://docs.genlayer.com/developers/intelligent-contracts/features/non-determinism).

## Development Workflow That Actually Works

### 1. Start in GenLayer Studio

For quick exploration, use [GenLayer Studio](https://studio.genlayer.com). It is the fastest way to write, deploy, and interact with an Intelligent Contract without setting up local infrastructure.

Use it to:

- Test contract syntax.
- Inspect state.
- Send simple reads and writes.
- Watch transaction behavior.
- Import deployed contracts by address.

### 2. Move to the Boilerplate for Serious Work

For a real project, use the [GenLayer Project Boilerplate](https://github.com/genlayerlabs/genlayer-project-boilerplate).

The official boilerplate is valuable because it gives a judge evidence that the builder understands the full development lifecycle:

```text
contracts/              Intelligent Contracts
tests/direct/           Fast in-memory tests
tests/integration/      Integration tests against GenLayer environments
frontend/               Frontend scaffold with GenLayerJS
deploy/                 Deployment scripts
gltest.config.yaml      Test network configuration
```

Recommended setup from the docs:

```bash
git clone https://github.com/genlayerlabs/genlayer-project-boilerplate.git
cd genlayer-project-boilerplate
pip install -r requirements.txt
```

### 3. Lint Before Testing

Run:

```bash
genvm-lint check contracts/my_contract.py
```

The linter helps catch problems such as:

- Forbidden imports.
- Non-deterministic calls outside the right context.
- Invalid storage types.
- Missing decorators.
- Missing return type annotations.

This matters because AI-generated code often looks plausible but violates GenVM rules. Linting is the first filter against that.

### 4. Test Direct Mode First

Direct mode is the fastest feedback loop. It runs contract code in memory without Docker or a full network.

Use it for:

- Constructor behavior.
- View method behavior.
- Write method behavior.
- Access control.
- Storage changes.
- Mocked web and LLM calls.
- Validator agreement and disagreement.

Example:

```python
def test_storage(direct_deploy):
    storage = direct_deploy("contracts/Storage.py", "initial value")

    assert storage.get_value() == "initial value"

    storage.set_value("updated")

    assert storage.get_value() == "updated"
```

For non-deterministic work, mock external calls:

```python
def test_price_feed(direct_vm, direct_deploy):
    direct_vm.mock_web(
        r"api\.example\.com/price",
        {"status": 200, "body": "{\"price\": 42.50}"},
    )

    contract = direct_deploy("contracts/PriceFeed.py")
    contract.update_price()

    assert contract.get_price() == 42.50
```

### 5. Use Studio Mode for Integration

After direct tests pass, use Studio Mode or another GenLayer environment to validate behavior through the actual RPC flow.

Use Studio Mode for:

- Multi-validator behavior.
- End-to-end transaction behavior.
- Debugging with logs.
- Pre-testnet confidence.

Official reference: [Testing Intelligent Contracts](https://docs.genlayer.com/developers/intelligent-contracts/testing).

## Network Strategy

GenLayer supports different environments for different stages of development.

| Network | Best Use |
| --- | --- |
| Localnet | Local development with full control. Default RPC: `http://localhost:4000/api`. |
| Studionet | Hosted development with no local setup. RPC: `https://studio.genlayer.com/api`. |
| Testnet Asimov | Infrastructure and stress testing. |
| Testnet Bradbury | Production-like testing with real AI/LLM workloads. RPC: `https://rpc-bradbury.genlayer.com`. |

Recommended flow:

1. Start on Studionet for speed.
2. Move to Localnet when you need full control and local debugging.
3. Use Testnet Bradbury for production-like testing with real AI workloads.

Official references:

- [Networks](https://docs.genlayer.com/developers/networks)
- [Network Configuration](https://docs.genlayer.com/developers/intelligent-contracts/deploying/network-configuration)

## Frontend Integration with GenLayerJS

The frontend should not guess contract methods. It must call the exact functions exposed by the deployed Intelligent Contract.

Reads use `readContract`:

```typescript
const result = await client.readContract({
  address: contractAddress,
  functionName: "get_data",
  args: [],
});
```

Writes use `writeContract` and then wait for a receipt:

```typescript
const txHash = await client.writeContract({
  address: contractAddress,
  functionName: "set_data",
  args: ["new value"],
});

const receipt = await client.waitForTransactionReceipt({
  hash: txHash,
  status: "ACCEPTED",
});
```

Important frontend rules:

- A transaction hash is not proof that the state changed. Always wait for the receipt.
- `ACCEPTED` is faster and useful for good UX after validator acceptance.
- `FINALIZED` is stronger and should be used when the app needs appeal-window finality.
- For browser dApps, use the wallet flow supported by GenLayerJS and call `client.connect()` before sending writes.
- Do not hardcode private keys in frontend code. If a private key is used for local scripts or prototypes, keep it outside public browser bundles.
- Always handle malformed JSON and unexpected contract return values.

Official references:

- [GenLayerJS SDK](https://docs.genlayer.com/developers/decentralized-applications/genlayer-js)
- [Reading from Intelligent Contracts](https://docs.genlayer.com/developers/decentralized-applications/reading-data)
- [Writing to Intelligent Contracts](https://docs.genlayer.com/developers/decentralized-applications/writing-data)

## A Strong AI-Builder Prompt for GenLayer Frontends

Use this prompt after you already have your Intelligent Contract or at least the exact contract method names.

```text
# GenLayer Frontend Builder Prompt

You are an expert TypeScript frontend developer building a GenLayer dApp.

Your job is to connect my frontend to an already-written GenLayer Intelligent Contract.

Critical rule:
Before writing code, ask me for the contract address, the contract source or ABI-equivalent method list, every public view method, every public write method, argument names, argument types, return shapes, and target network. Do not invent method names such as get_data, getWord, generateNewWord, mint, or update_storage unless they exist in my contract.

Protocol context:
- GenLayer Intelligent Contracts are Python contracts executed by GenVM.
- View methods decorated with @gl.public.view are read-only and should be called with readContract.
- Write methods decorated with @gl.public.write modify state and should be called with writeContract.
- Write calls return a transaction hash first. The UI must wait for waitForTransactionReceipt before treating the operation as successful.
- Use ACCEPTED for normal user-facing confirmation unless I specifically ask for FINALIZED.
- Use FINALIZED when the feature must wait for the finality window.

Implementation requirements:
- Use genlayer-js.
- Use the correct network: localnet, studionet, testnetAsimov, or testnetBradbury, based on my answer.
- If this is a browser dApp, use the wallet connection flow supported by GenLayerJS and call client.connect() before writes.
- Do not hardcode private keys in browser code.
- If this is a local script or backend-only prototype, keep private keys in server-side environment variables only.
- Build one reusable GenLayer client module.
- Build typed wrapper functions for every contract read and write.
- Add receipt waiting for every write transaction.
- Add structured transaction states: idle, preparing, wallet-confirmation, submitted, accepted, finalized, failed.
- Add useful errors for wrong network, rejected signature, timeout, insufficient funds, missing contract address, function not found, and malformed return data.
- If a contract returns JSON strings, parse them inside try/catch and show a safe fallback instead of crashing the UI.
- If a contract returns objects, render unknown keys dynamically rather than assuming only one field exists.

Do not:
- Do not guess function names.
- Do not assume the contract returns { word, definition } unless I provide that schema.
- Do not treat a transaction hash as success.
- Do not wait forever without timeout or user feedback.
- Do not put VITE_PRIVATE_KEY or any private key into client-side code.
- Do not hide transaction errors from the user.

Deliver:
- A GenLayer client module.
- Typed contract wrapper functions.
- A UI component that shows read state, write progress, receipt status, and errors.
- A short checklist for testing on the chosen network.
```

## Example Frontend Wrapper Pattern

This is a generic shape, not a copy-paste replacement for every project. The function names must match the deployed contract.

```typescript
import { createClient } from "genlayer-js";
import { studionet } from "genlayer-js/chains";

export const contractAddress = import.meta.env.VITE_CONTRACT_ADDRESS as `0x${string}`;

export function createGenLayerClient(walletAddress: `0x${string}`) {
  return createClient({
    chain: studionet,
    account: walletAddress,
  });
}

export async function connectStudionet(client: ReturnType<typeof createGenLayerClient>) {
  await client.connect("studionet");
}

export async function readContractData(client: ReturnType<typeof createGenLayerClient>) {
  return client.readContract({
    address: contractAddress,
    functionName: "REPLACE_WITH_REAL_VIEW_METHOD",
    args: [],
  });
}

export async function writeContractData(
  client: ReturnType<typeof createGenLayerClient>,
  value: string,
) {
  const hash = await client.writeContract({
    address: contractAddress,
    functionName: "REPLACE_WITH_REAL_WRITE_METHOD",
    args: [value],
  });

  return client.waitForTransactionReceipt({
    hash,
    status: "ACCEPTED",
    interval: 5_000,
    retries: 60,
  });
}
```

Why placeholders are used here:

- A frontend wrapper that guesses the function name is fragile.
- A strong GenLayer dApp starts from the contract source or exact deployed method list.
- The wrapper should be generated after the contract interface is known.

## Debugging Checklist

When a GenLayer project fails, check in this order:

1. Contract syntax and decorators.
2. Linter results from `genvm-lint check`.
3. Whether `gl.nondet.*` calls are inside non-deterministic blocks.
4. Whether storage writes happen after consensus, not inside leader or validator functions.
5. Whether validator logic returns `True` or `False` clearly.
6. Whether the app is waiting for the correct status: `ACCEPTED` or `FINALIZED`.
7. Whether the frontend is using the correct network.
8. Whether the contract address is correct for that network.
9. Whether the frontend is calling real contract functions, not guessed names.
10. Whether the return data is parsed safely.
11. Whether the wallet is connected to the expected GenLayer network.
12. Whether GenLayer Studio logs show input validation errors, contract logic errors, non-deterministic output issues, or consensus errors.

Official reference: [Debugging Intelligent Contracts](https://docs.genlayer.com/developers/intelligent-contracts/debugging).

## What Makes a GenLayer Project High Quality

A strong GenLayer submission should make these qualities easy to verify:

| Category | High-Quality Signal |
| --- | --- |
| Protocol understanding | Explains GenVM, Intelligent Contracts, Optimistic Democracy, and the Equivalence Principle accurately. |
| Contract design | Uses `@gl.public.view` and `@gl.public.write` correctly and keeps non-deterministic logic inside the right blocks. |
| Consensus design | Chooses strict equality, custom validator functions, comparative validation, or non-comparative validation based on the use case. |
| Safety | Handles bad API responses, malformed LLM output, validator disagreement, timeouts, and user-facing errors. |
| Testing | Uses direct tests first, mocks LLM/web calls, and adds Studio/integration tests where consensus behavior matters. |
| Frontend quality | Uses GenLayerJS reads and writes correctly, waits for receipts, and does not guess contract functions. |
| Documentation | Gives a path from concept to deployable dApp, with references to official docs and working project examples. |
| Reusability | Provides prompts, checklists, and patterns other builders can actually use. |

## My Builder Thesis

The main lesson from building with GenLayer is that the hard part is not only writing Python or connecting a frontend.

The hard part is designing the equivalence rule.

If the data is objective, normalize it and use strict equality. If the data drifts, use a tolerance. If the output is structured, compare the fields that matter. If the output is subjective, use comparative or non-comparative validation with clear criteria. If the result is ambiguous, let the transaction fail or become undetermined instead of forcing bad state into the contract.

That is the difference between a demo that only works once and a GenLayer application that can survive real AI and web data.

## Official Docs Used

- [Introduction to Intelligent Contracts](https://docs.genlayer.com/developers/intelligent-contracts/introduction)
- [GenVM](https://docs.genlayer.com/understand-genlayer-protocol/core-concepts/genvm)
- [Optimistic Democracy](https://docs.genlayer.com/understand-genlayer-protocol/core-concepts/optimistic-democracy)
- [The Equivalence Principle](https://docs.genlayer.com/developers/intelligent-contracts/equivalence-principle)
- [Non-determinism](https://docs.genlayer.com/developers/intelligent-contracts/features/non-determinism)
- [Development Setup](https://docs.genlayer.com/developers/intelligent-contracts/tooling-setup)
- [Testing Intelligent Contracts](https://docs.genlayer.com/developers/intelligent-contracts/testing)
- [Debugging Intelligent Contracts](https://docs.genlayer.com/developers/intelligent-contracts/debugging)
- [GenLayer Studio](https://docs.genlayer.com/developers/intelligent-contracts/tools/genlayer-studio)
- [Deploy Contracts in GenLayer Studio](https://docs.genlayer.com/developers/intelligent-contracts/tools/genlayer-studio/deploying-contract)
- [GenLayerJS SDK](https://docs.genlayer.com/developers/decentralized-applications/genlayer-js)
- [Reading from Intelligent Contracts](https://docs.genlayer.com/developers/decentralized-applications/reading-data)
- [Writing to Intelligent Contracts](https://docs.genlayer.com/developers/decentralized-applications/writing-data)
- [Networks](https://docs.genlayer.com/developers/networks)
- [Network Configuration](https://docs.genlayer.com/developers/intelligent-contracts/deploying/network-configuration)
- [gen_getContractSchema](https://docs.genlayer.com/api-references/genlayer-node/gen/gen_getContractSchema)
