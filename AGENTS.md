# Repository Agent Instructions

This file applies to the entire repository. It is the entry point for coding
agents, not a replacement for the detailed repository guidance.

## Read before changing anything

1. Read `.agents/skills/CMSC495-CAP/SKILL.md` for the stack, layout,
   conventions, validation requirements, and known traps.
2. Read `CONTRIBUTING.md` for the supported setup and contribution workflow.
3. Read the relevant section of `README.md` before changing behavior or
   architecture.
4. When documentation and code disagree, verify the code and correct the
   documentation in the same pull request.

## Required workflow

1. Synchronize from the current upstream `main`.
2. Work in an isolated branch or worktree.
3. Keep each contribution focused and reviewable.
4. Run `make check` before requesting review.
5. Use the repository pull request template and explain what changed, why it
   changed, and how it was verified.
6. Obtain peer review. Only an authorized maintainer merges into the team
   repository.

Branch names use a purpose prefix such as `feat/`, `fix/`, `docs/`,
`test/`, or `ci/`. Commit and pull request subjects use
`type: concise description`.

## Security rules

1. Never commit `.env`, API keys, access keys, private SSH keys, password
   hashes, real policy documents, or other secrets.
2. Keep `.env.example` limited to blank values and safe placeholders.
3. Actual deployment secrets belong only in the untracked EC2 `.env`.
4. Public SSH keys may be shared with the EC2 administrator. Private SSH keys
   must never be transmitted.
5. Do not weaken authentication, grounding, refusal, citation, rate limiting,
   or server controlled history protections to make a test pass.

## AI behavior rules

1. Conversation history remains server controlled. The client sends a question
   and session identifier, not trusted history.
2. Unsupported questions must pass through the grounding gate and refuse rather
   than guess.
3. Every supported answer must retain its source evidence.
4. Prompt changes require a `PROMPT_VERSION` update.
5. Changes to answer or refusal behavior require evaluation coverage, or a clear
   explanation in the pull request.
6. Stub results demonstrate application behavior, not live model quality.
7. Vendor specific model code belongs in `src/llm.py`.

## Architecture boundary

Caddy is the only publicly exposed service. Caddy terminates TLS and forwards
to the frontend Nginx service. Nginx serves the React application and proxies
API requests to FastAPI. MongoDB Atlas, Amazon S3, and OpenAI remain external
managed services reached only by the backend.

Ports 3000 and 8000 must not be published from the production EC2 security
group or Docker Compose configuration.

## Completion standard

A change is complete only when its focused tests pass, `make check` passes,
documentation reflects the resulting behavior, no secret or generated artifact
entered the diff, and a reviewer can reproduce the verification steps.
