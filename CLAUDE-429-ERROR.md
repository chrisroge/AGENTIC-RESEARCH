You’re hitting Bedrock’s **token-per-minute throttle (HTTP 429 “Too many tokens”)**, not a Claude Code bug. Bedrock counts **input + output + thinking tokens** against your minute bucket; when Claude Code does repo analysis / tool runs, it can burst past the small default TPM/RPM quotas—especially in GovCloud. GitHub issues and AWS posts show the exact 429 string you’re seeing, even on first use, when the token bucket is empty or small.

Two things in your config amplify the problem:

1. **You set the “small/fast” model to Sonnet 3.7** (same as primary). Background tasks that should be cheap (Haiku) now consume Sonnet-class tokens.

2. **`MAX_THINKING_TOKENS=1024`** turns on extended thinking for *every* request in Claude Code, which burns extra tokens per call. (There’s even an open issue about this behavior.) ([GitHub][2])

Also, lowering `CLAUDE_CODE_MAX_OUTPUT_TOKENS` below 4096 **won’t help** on Bedrock; Anthropic’s Claude Code docs explain Bedrock’s “burndown throttling” still penalizes you as if 4096 were set. ([Claude Docs][3])

---

# What to change now (minimal friction)

## 1) Fix the model mix (make background work cheap)

Use Haiku for the “small/fast” slots, and keep Sonnet 3.7 only for foreground:

```json
{
  "env": {
    "CLAUDE_CODE_USE_BEDROCK": "1",
    "AWS_REGION": "us-gov-west-1",
    "AWS_PROFILE": "bedrock",

    "ANTHROPIC_MODEL": "anthropic.claude-3-7-sonnet-20250219-v1:0",

    // Use Haiku in GovCloud for background ops:
    "ANTHROPIC_SMALL_FAST_MODEL": "anthropic.claude-3-haiku-20240307-v1:0",
    "ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION": "us-gov-west-1",

    // Keep Bedrock-friendly defaults:
    "CLAUDE_CODE_MAX_OUTPUT_TOKENS": "4096",
    // Disable global extended thinking unless you truly need it:
    // remove MAX_THINKING_TOKENS entirely, or set "256" for occasional depth
    // "MAX_THINKING_TOKENS": "0",

    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1"
  }
}
```

(Claude’s Bedrock guide recommends 4096 output and 1024 thinking for *commercial* regions; in GovCloud with tighter TPM, start with thinking **off** and enable only when needed.)

## 2) Trim input tokens so each call is lighter

Give Claude Code a deny-list so it won’t slurp huge dirs (node\_modules, build, logs, .git):

```json
{
  "permissions": {
    "deny": [
      "Read(./node_modules/**)",
      "Read(./.git/**)",
      "Read(./dist/**)",
      "Read(./build/**)",
      "Read(./.next/**)",
      "Read(./coverage/**)",
      "Read(./**/*.log)"
    ]
  }
}
```

(That’s the supported way to exclude files in `.claude/settings.json`.)

## 3) Prefer “write to files, summarize in chat”

Long outputs (scaffolds, test reports) should be **written via the Write tool** and summarized briefly in chat. This keeps output tokens low while still producing full artifacts. (This is a recommended pattern in Claude Code best-practice material.)

---

# Capacity fixes (to stop 429s without “keep going”)

* **Request a quota increase** in **Service Quotas → Amazon Bedrock** for:

  * *On-demand model inference tokens per minute* for **Anthropic Claude 3.7 Sonnet v1** (your Region).
  * Optionally *Requests per minute* too.

  Bedrock’s docs spell out the TPM/RPM limits and how they’re enforced; many teams hit 429 until quotas are raised.

* If you need guaranteed throughput (no throttling during demos/CI): consider **Provisioned Throughput (model units)** for the model; it buys you guaranteed TPM.

> Tip: In GovCloud, model availability/authorizations differ from commercial regions. Double-check model access and quotas for **us-gov-west-1** with `list-inference-profiles` or the GovCloud Bedrock page if anything looks off.

---

# Quick checklist

* [ ] Switch `ANTHROPIC_SMALL_FAST_MODEL` → **Haiku** (GovCloud ID).
* [ ] **Remove or lower** `MAX_THINKING_TOKENS` (enable per task when needed). ([GitHub][2])
* [ ] Add **permissions.deny** to cut context bloat. ([Claude Docs][4])
* [ ] **Request TPM/RPM quota increases** (and/or Provisioned Throughput). ([AWS Documentation][6])

That combo typically eliminates the “Too many tokens” wall while keeping flows fully automated (no “keep going” prompts).

[1]: https://github.com/anthropics/claude-code/issues/77?utm_source=chatgpt.com "claude code not working on Mac Os with Bedrock · Issue #77"
[2]: https://github.com/anthropics/claude-code/issues/5257?utm_source=chatgpt.com "[BUG] MAX_THINKING_TOKENS forces every request to ..."
[3]: https://docs.claude.com/en/docs/claude-code/amazon-bedrock "Claude Code on Amazon Bedrock - Claude Docs"
[4]: https://docs.claude.com/en/docs/claude-code/settings "Claude Code settings - Claude Docs"
[5]: https://www.anthropic.com/engineering/claude-code-best-practices?utm_source=chatgpt.com "Claude Code: Best practices for agentic coding"
[6]: https://docs.aws.amazon.com/bedrock/latest/userguide/quotas.html?utm_source=chatgpt.com "Quotas for Amazon Bedrock"
[7]: https://aws.amazon.com/bedrock/pricing/?utm_source=chatgpt.com "Amazon Bedrock pricing"
[8]: https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/govcloud-bedrock.html?utm_source=chatgpt.com "Amazon Bedrock in AWS GovCloud (US)"


## Updated settings.json
```json
{
  "env": {
    "CLAUDE_CODE_USE_BEDROCK": "1",
    "AWS_REGION": "us-gov-west-1",
    "AWS_PROFILE": "bedrock",
    "ANTHROPIC_MODEL": "anthropic.claude-3-7-sonnet-20250219-v1:0",
    "ANTHROPIC_SMALL_FAST_MODEL": "anthropic.claude-3-haiku-20240307-v1:0",
    "ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION": "us-gov-west-1",
    "CLAUDE_CODE_MAX_OUTPUT_TOKENS": "4096",
    "MAX_THINKING_TOKENS": "0",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1"
  },
  "permissions": {
    "deny": [
      "Read(./node_modules/**)",
      "Read(./.git/**)",
      "Read(./dist/**)",
      "Read(./build/**)",
      "Read(./.next/**)",
      "Read(./coverage/**)",
      "Read(./.cache/**)",
      "Read(./**/*.log)",
      "Read(./.venv/**)",
      "Read(./.terraform/**)"
    ]
  }
}
json```
