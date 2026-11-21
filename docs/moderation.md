# Community moderation

This document explains how the community registry reviews, approves, and
removes adapters. Moderation protects students and maintainers without turning
review into gatekeeping. See [inclusion.md](inclusion.md) for the conduct and
accessibility expectations that apply to every review.

## Scope

Moderation covers:

- adapter code, manifests, README files, and any bundled assets;
- changes to `registry.json` (the generated index);
- changes to signing, schemas, workflows, or key material, which follow the
  stricter code-owner path in `CODEOWNERS`;
- reports about published adapters, including security reports and conduct
  reports.

Out of scope: disputes about grades, course policy, or the behavior of the app
outside the adapter sandbox.

## Roles

| Role | Responsibility |
| --- | --- |
| Contributor | Opens a pull request, responds to review, keeps the adapter current. |
| Reviewer | Checks safety, correctness, least privilege, and documentation. Any maintainer may review. |
| Code owner | Required approval for `schemas/`, `scripts/`, `keys/`, `.github/`, `registry.config.json`, `registry.json`, `SECURITY.md`, and the policy docs. |
| Release signer | Merges approved changes, then runs the protected signing workflow. The signer does not bypass review. |
| Moderator | Handles conduct reports, conflicts of interest, and removal decisions. |

Reviews are done in public pull requests unless the report is a security or
conduct matter. A reviewer with a conflict of interest (for example, the author
of the adapter or an employer with a competing adapter) must disclose it and
recuse themselves from the decision.

## Submission flow

1. **Open a pull request.** Use the adapter template and include the manifest,
   `adapter.lua`, README, tests where useful, and the regenerated
   `registry.json`.
2. **Automated checks.** `Validate registry` verifies canonical JSON, schema,
   folder/id agreement, mirrored manifest fields, file hashes, README versions,
   and runtime defaults. `Sign dry run` signs a throwaway copy. A failure must
   be fixed before human review; it is not a moderation decision.
3. **Triage (target: 5 business days).** A maintainer confirms the submission is
   in scope, asks for missing information, and applies a review label.
4. **Technical review.** Reviewers check the items in the checklist below.
   Security-sensitive behavior may need a second reviewer.
5. **Decision.** Approve, request changes, or decline with a reason. Silence is
   not approval. At least one maintainer approval is required, and code owners
   must approve security-sensitive paths.
6. **Merge and sign.** After merge, the signing workflow enters
   `registry-signing-community`, signs the exact bytes, and publishes a new
   `registry-v<sequence>` tag.
7. **Post-merge.** The contributor is listed in the pull request history. The
   adapter remains published until a later sequence removes or replaces it.

## Review checklist

A reviewer should be able to answer yes to each item:

- The adapter does what its name and README say, and fails safely.
- It requests only the permissions, extensions, directories, runtimes, and
  settings it actually uses.
- It does not read files, environment variables, or network destinations it did
  not declare.
- It does not hide behavior in generated, minified, or binary files; generated
  files must be reproducible and reviewed.
- Timeouts and errors produce no score or a clear error, never a silent zero.
- The manifest version, README version line, and `registry.json` entry agree.
- Every runtime file is listed with a correct SHA-256 digest.
- The README explains setup, expected output, and any runtime requirement.
- The change contains no secret, credential, student data, or personal data.
- The license and attribution are compatible with the repository's MIT license.

Reviewers do not require a particular author, institution, language, or
framework. They do require evidence that the code is safe and understandable.

## Decision labels

| Label | Meaning |
| --- | --- |
| `review: needs-info` | The submission is incomplete; the contributor has the next action. |
| `review: changes-requested` | Specific code or documentation changes are needed. |
| `review: security` | A security-focused reviewer must approve before merge. |
| `review: approved` | At least one maintainer approved; code-owner approval is still required where applicable. |
| `review: declined` | The change will not be merged; the reason is recorded in the pull request. |

## Removal and revocation

An adapter may be removed, or a new sequence may stop publishing it, when:

- it is malicious, unsafe, or no longer builds or runs;
- it infringes a license or privacy right;
- its author asks for removal;
- it repeatedly fails review or its maintainer is unresponsive and the adapter
  is broken.

Removal is a normal reviewed pull request that deletes the adapter folder and
regenerates `registry.json`. The signing workflow then publishes a new
sequence. Existing installations may keep the old signed sequence until the app
refreshes; the registry cannot retroactively erase a signed tag, so security
removals should also be announced in the repository and, for a key compromise,
handled through [key-rotation.md](key-rotation.md).

## Appeals and disputes

A contributor may ask for a second reviewer or a moderator decision. The
appeal should reference the pull request, the specific decision, and any new
evidence. Moderators aim to answer within 10 business days. Retaliation for an
appeal is a conduct violation.

## Conduct reports

Report harassment, discrimination, doxxing, or other conduct problems
privately to the maintainers listed in `CODEOWNERS`. Do not include student
data or secrets. Maintainers will acknowledge the report, avoid assigning
anyone with a conflict, and keep the reporter's identity confidential where
practical. Consequences range from a private warning to removal from the
project, and are documented in [inclusion.md](inclusion.md).

## Transparency

Moderation decisions, except security and conduct details, are recorded in the
pull request. Maintainers publish a short summary when an adapter is removed
for cause. Decisions should cite the checklist or policy they rely on so future
contributors can learn from them.
