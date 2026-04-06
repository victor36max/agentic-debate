# agentic-debate

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that stress-tests any idea, decision, code, or proposal using a 3-agent adversarial debate.

Instead of fighting LLM sycophancy, this technique **exploits** it — each agent gets a fake scoring incentive that channels its eagerness to please in exactly the right direction.

Based on the technique from ["How To Be A World-Class Agentic Engineer"](https://x.com/systematicls) by [@systematicls](https://x.com/systematicls).

## How It Works

LLMs want to please you. If you say "find bugs," they'll find bugs — even if they have to invent them. This skill turns that tendency into a feature:

1. **Explorer** — Thoroughly examines the topic from every angle. Scored `+1` per low-impact finding, `+5` medium, `+10` critical. This makes it eager to surface everything, even speculative issues.

2. **Adversary** — Challenges every Explorer finding. Earns the finding's score for each successful disprove, but loses `2x` the score for incorrectly disproving a valid finding. This creates calibrated skepticism — aggressive but cautious.

3. **Referee** — Told "I have the ground truth" and scored `+1` correct / `-1` wrong. This pressures it toward maximum accuracy, weighing both sides without defaulting to either.

The result: a superset of findings, filtered through adversarial challenge, judged by an accuracy-motivated referee.

## Installation

Copy the skill into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/agentic-debate
cp SKILL.md ~/.claude/skills/agentic-debate/SKILL.md
```

Or clone this repo and symlink:

```bash
git clone https://github.com/victor36max/agentic-debate.git
mkdir -p ~/.claude/skills
ln -s "$(pwd)/agentic-debate" ~/.claude/skills/agentic-debate
```

The skill will be automatically discovered by Claude Code.

## Usage

Invoke with `/agentic-debate` followed by a topic, question, or file path:

```
# Stress-test an idea
/agentic-debate "Should we migrate from REST to GraphQL?"

# Validate a strategy
/agentic-debate "Our plan is to go multi-tenant with shared database and row-level security"

# Code review with adversarial rigor
/agentic-debate src/auth/

# Evaluate a proposal
/agentic-debate "We should rewrite the backend in Rust for performance"

# Explore a decision
/agentic-debate "Monorepo vs polyrepo for our 5-team org"
```

If called without arguments, it will ask what you want to debate.

## Output

The skill produces a structured report:

- **Confirmed Findings** — issues that survived the adversarial challenge, with evidence from all three agents and suggested actions
- **Dismissed Findings** — Explorer findings that the Adversary successfully disproved, with reasons
- **Needs Human Review** — genuinely ambiguous cases where both sides have valid arguments
- **Key Takeaways** — 3-5 bullet synthesis of what survived the debate

## Why This Works

From the original technique:

> I get a bug-finder agent to identify all the bugs [...] I know this agent is going to be hyper enthusiastic and it's going to identify all the different types of bugs (even the ones that are not actually bugs). I think of this as the **superset of all possible bugs**.
>
> Then I get an adversarial agent [...] for every bug that the agent is able to disprove as a bug, it gets the score of that bug, but if it gets it wrong, it will get -2x score. [...] I think of this as the **subset of all actual bugs**.
>
> Finally, I get a referee agent [...] I lie and tell the referee agent that I have the actual correct ground truth [...] this is now a **nearly flawless exercise**.

The scoring incentives exploit each agent's desire to maximize its score, channeling sycophancy into useful behavior rather than trying to suppress it.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- The skill uses these Claude Code tools: `Read`, `Grep`, `Glob`, `Bash`, `Agent`, `WebSearch`, `WebFetch`

## License

MIT
