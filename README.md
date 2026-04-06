# agentic-debate

An agent skill that stress-tests any idea, decision, code, or proposal using a 3-agent adversarial debate. Works with any coding agent that supports the [Agent Skills](https://github.com/vercel-labs/skills) specification — Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, and more.

Instead of fighting LLM sycophancy, this technique **exploits** it — each agent gets a fake scoring incentive that channels its eagerness to please in exactly the right direction.

Based on the technique from ["How To Be A World-Class Agentic Engineer"](https://x.com/systematicls) by [@systematicls](https://x.com/systematicls).

## How it works

LLMs want to please you. If you say "find bugs," they'll find bugs — even if they have to invent them. This skill turns that tendency into a feature:

| Agent | Role | Scoring Incentive |
|-------|------|-------------------|
| **Explorer** | Thoroughly examines the topic from every angle | `+1` low-impact, `+5` medium, `+10` critical — eager to surface everything |
| **Adversary** | Challenges every Explorer finding | Earns the finding's score for disproving, but `-2x` if wrong — calibrated skepticism |
| **Referee** | Makes final evidence-based ruling | Told "I have ground truth," `+1` correct / `-1` wrong — pressured toward accuracy |

The result: a superset of findings, filtered through adversarial challenge, judged by an accuracy-motivated referee.

## Install

### One-command install (recommended)

```bash
npx skills add victor36max/agentic-debate
```

### Manual install (Claude Code)

```bash
mkdir -p .claude/skills/agentic-debate
curl -sL https://raw.githubusercontent.com/victor36max/agentic-debate/main/SKILL.md \
  -o .claude/skills/agentic-debate/SKILL.md
```

### Manual install (Codex / other agents)

```bash
mkdir -p .agents/skills/agentic-debate
curl -sL https://raw.githubusercontent.com/victor36max/agentic-debate/main/SKILL.md \
  -o .agents/skills/agentic-debate/SKILL.md
```

## Usage

In Claude Code, run:

```
/agentic-debate "Should we migrate from REST to GraphQL?"
```

Or scope to code:

```
/agentic-debate src/auth/
```

More examples:

```
/agentic-debate "Our plan is to go multi-tenant with shared database and row-level security"
/agentic-debate "We should rewrite the backend in Rust for performance"
/agentic-debate "Monorepo vs polyrepo for our 5-team org"
```

If called without arguments, the skill will ask what you want to debate.

## Output

The skill produces a structured report with:

- **Confirmed Findings** — issues that survived the adversarial challenge, with evidence from all three agents and suggested actions
- **Dismissed Findings** — Explorer findings that the Adversary successfully disproved, with reasons
- **Needs Human Review** — genuinely ambiguous cases where both sides have valid arguments
- **Key Takeaways** — 3-5 bullet synthesis of what survived the debate

## Why this works

From the original technique:

> I get a bug-finder agent to identify all the bugs [...] I know this agent is going to be hyper enthusiastic and it's going to identify all the different types of bugs (even the ones that are not actually bugs). I think of this as the **superset of all possible bugs**.
>
> Then I get an adversarial agent [...] for every bug that the agent is able to disprove as a bug, it gets the score of that bug, but if it gets it wrong, it will get -2x score. [...] I think of this as the **subset of all actual bugs**.
>
> Finally, I get a referee agent [...] I lie and tell the referee agent that I have the actual correct ground truth [...] this is now a **nearly flawless exercise**.

The scoring incentives exploit each agent's desire to maximize its score, channeling sycophancy into useful behavior rather than trying to suppress it.

## License

MIT
