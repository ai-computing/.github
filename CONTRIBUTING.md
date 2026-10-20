# Contributing to AI Computing repositories

This is the default guide for every repository in the organization. A repository's own `CONTRIBUTING.md`
adds to it (build commands, hardware checks) and wins where they differ.

We use **GitHub flow**: one long-lived `main` (or `master`) branch, and a short-lived branch per change.

```
Issue → branch → (draft) PR → review + CI → merge (squash) → delete branch → release when needed
```

- An **Issue** says what will be solved and when it counts as done.
- A **PR** says what changed and shows the evidence that it works.
- A **merge** happens only when the checks below are met.

## 1. Issues

Open an Issue for a bug, a performance problem, a feature, a new backend or an API/file-format change.
Discuss the design in the Issue first when the change is large. Small fixes (a typo, a broken link,
an obvious one-liner) can go straight to a PR. Usage questions go to Discussions or the maintainers.

Write five things (the templates ask for them):

1. **Problem**: what is wrong or missing, and in which situation.
2. **Evidence**: how to reproduce it, logs, measurements.
3. **Scope**: what this work will and will not cover.
4. **Done when**: the result that lets us close it, for example "existing correctness tests pass; latency
   compared before/after on the agreed inputs; commands and results recorded; regressions explained".
5. **Owner**: who does the next step.

For research, open one Issue per hypothesis and link the individual runs to it, not one Issue per run.

States: Backlog → Ready → In progress → In review → Done. Labels: `bug`, `feature`, `performance`, `docs`,
`help wanted`, `good first issue`. Issues we will not take are closed with the reason.

## 2. Branches

- Name a branch after its purpose and Issue: `fix/42-card-ordinal`, `feat/57-nixl-transport`,
  `perf/42-distance-kernel`, `docs/61-roadmap`, `exp/63-cross-node-pd`.
- Branch from the latest default branch. Never push to the default branch directly.
- People or agents working at the same time use separate clones or `git worktree`s, not shared checkouts.
  Express machine differences as config, not as long-lived per-server branches.

## 3. Pull requests

- **One reviewable purpose per PR.** Keep formatting sweeps, API changes, dependency bumps and the actual
  change apart. A fix and its regression test belong together. Long work can start as a **Draft PR**.
- Fill in the PR template: Background, Changes, Verification, Compatibility and limits, Related Issue.
- Use `Closes #N` only when the PR finishes the Issue; otherwise `Related to #N` and leave the rest in the Issue.
- Verification fits the change:

| Change | Evidence in the PR |
|---|---|
| Docs | content, links and rendering checked |
| Bug fix | the failure reproduced, then gone after the fix (ideally a regression test) |
| Feature | main behaviour and edge cases tested |
| Performance | correctness kept, and before/after under the same conditions |
| API or file format | effect on existing users and how to migrate |

- **Performance or hardware results** record: baseline and new commit SHAs; hardware (CPU/GPU/NPU, memory),
  driver and SDK/compiler versions; model, dataset, input sizes, batch and key settings; correctness criterion
  (tolerance, token-exactness); method (warm-up, repetitions, median/mean); the metrics (latency, throughput,
  memory, energy); and the command, config and result files to reproduce it. Keep code, scripts, configs and
  small summaries in the repository; keep large datasets, models and raw logs elsewhere and link them by
  version, hash or run ID.
- Expensive GPU/NPU runs are not required on every PR: fast checks run in CI, and the owner runs hardware
  checks for the changes that need them and attaches the results.

## 4. Review and merge

- Reviewers check correctness, scope, compatibility with existing behaviour and maintainability, and mark
  each comment as **must fix** or **suggestion**. Authors answer every comment: fixed, or why not.
  Fixes go on the same PR branch.
- Merge only when **all required checks are green**, the review conversations are resolved, and the Issue's
  "done when" is met (or the remaining part is written down in the Issue).
- Default merge method: **Squash and merge** (one PR = one commit on the default branch). Delete the branch
  afterwards and start the next change from the updated default branch.
- If a merged change has to be undone, revert it with a PR and record the cause and follow-up in the Issue.
- Repository settings: squash merge is the only merge method and merged branches are deleted automatically.
  The default branch takes changes through PRs only, never by direct or force push. This is the rule, not yet
  enforced by a branch ruleset.

## 5. Releases

When a state is ready for others to install or to reproduce results with, tag the verified commit
(`vX.Y.Z`) and write a GitHub Release: what is included and changed, how to install and run, the hardware
and software it was verified on, known limits, and the configuration to reproduce papers or results.

## 6. Security and data

Public repositories must not contain IP addresses, internal hostnames, tokens, passwords, kubeconfigs or
site-specific paths. Use placeholders or TEST-NET addresses (192.0.2.x), keep site values in untracked
files, and run the repository's sensitive-data check (`python tools/check_sensitive.py` where it exists)
before pushing. Report vulnerabilities privately to the maintainers, not in a public Issue.

## 7. AI-assisted work

AI agents follow the same flow: they work on an Issue with an agreed scope, in their own branch or worktree,
run the repository's checks, put the verification evidence in the PR, and leave the merge to the maintainer.
PRs written with AI assistance say so (for example the "Generated with Claude Code" footer). A person or the
lead agent reviews and verifies them before merge.

When contributing to other open-source projects, follow that project's `CONTRIBUTING.md` first (DCO
sign-off, CLA, AI-use policy, PR title format), search existing Issues and PRs, discuss large changes before
implementing, and work from a fork if you lack write access.

## 8. Engine boundary

Optimus Prime (`aicomp`) is maintained by its owner. Changes needed in the engine are filed there as
feature-request Issues with the evidence and acceptance criteria; they are not patched from other repositories.

---

## 한국어 요약

모든 저장소의 기본 기여 절차입니다. 저장소에 자체 `CONTRIBUTING.md`가 있으면 그것이 우선합니다.

- **흐름**: Issue → 작업 브랜치 → (Draft) PR → 리뷰와 CI → Squash and merge → 브랜치 삭제 → 필요할 때 Release.
- **Issue**: 버그·성능·기능·API 변경은 Issue부터 씁니다. 문제, 근거, 범위, 완료 조건, 담당자를 적습니다. 오탈자 같은 작은 수정은 PR로 바로 시작해도 됩니다. 연구는 가설 하나를 Issue 하나로 잡고 실행 결과를 연결합니다.
- **브랜치**: `fix/42-...`, `feat/...`, `perf/...`, `docs/...`, `exp/...`처럼 목적과 Issue 번호로 만들고, 기본 브랜치에 직접 push하지 않습니다. 동시에 작업하는 사람·에이전트는 별도 clone이나 worktree를 씁니다.
- **PR**: 목적 하나만 담습니다. 템플릿의 배경, 변경 내용, 검증, 호환성과 제약, 관련 Issue를 채웁니다. Issue를 끝낼 때만 `Closes #N`, 일부만 해결하면 `Related to #N`을 씁니다. 성능·하드웨어 결과에는 commit SHA, 환경, 입력, 정확성 기준, 측정 방법, 결과, 재현 명령을 남깁니다.
- **Merge**: 필수 검사가 모두 통과하고, 리뷰 대화가 해결되고, Issue 완료 조건이 충족됐을 때만 합칩니다. 기본은 Squash and merge입니다. 되돌릴 때도 PR로 남깁니다.
- **Release**: 검증한 커밋에 태그를 달고 기능, 설치·실행 방법, 검증 환경, 알려진 제약, 재현 설정을 적습니다.
- **보안**: 공개 저장소에 IP, 내부 호스트명, 토큰, 비밀번호, kubeconfig를 올리지 않고 push 전에 민감 정보 검사를 실행합니다.
- **AI 작업**: 같은 절차를 따르고, PR에 검증 근거와 AI 사용 여부를 남기며, Merge는 관리자가 합니다.
- **엔진 경계**: Optimus Prime(`aicomp`)은 다른 저장소에서 고치지 않고 기능 요청 Issue로 올립니다.
