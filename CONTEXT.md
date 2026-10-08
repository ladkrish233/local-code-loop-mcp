# local-code-loop-mcp

An MCP server that generates code against a locally-hosted model, iterating against an automated check pipeline until it passes or gives up.

## Language

**Target repo**:
The repository the MCP server generates code into and runs checks against, identified by the client's working directory and configured via its own `.local-code-loop.json`.
_Avoid_: project, workspace, repo (alone, when ambiguous with local-code-loop-mcp itself)

**Code task**:
One request to generate code against the target repo; it is iterated through the check pipeline until it passes or the iteration cap is hit.
_Avoid_: job, request, generation

**Adapter**:
A backend-specific implementation of the model-client interface: sends a prompt to a locally-hosted model and returns its completion text as a plain string. Ollama is the first adapter; the interface must stay provable with exactly one.
_Avoid_: backend, provider, connector, client (alone)

**Check**:
A single pass/fail step in the check pipeline: a shell command, with the exit code deciding pass or fail. Syntax, lint, test, and guardrail checks are all the same shape.
_Avoid_: test (as the general term), validator, step

**Guardrail**:
A check that enforces a repo-specific constraint beyond syntax/lint/test (e.g. no hardcoded secrets). Expressed and run identically to any other check — no guardrail-specific logic.
_Avoid_: policy, rule

**Check pipeline**:
The ordered set of checks configured for a target repo, run against a code task's generated output on each iteration.
_Avoid_: test suite, pipeline (alone)

**Iteration cap**:
The maximum number of generate-then-check cycles a code task may run before it is abandoned.
_Avoid_: max retries, retry limit

**Structured failure report**:
The machine-readable result returned for a code task that exhausted its iteration cap without passing: which checks failed on the last attempt, and the last generated output.
_Avoid_: error, failure log, exception
