# Inclusion and community conduct

The community registry exists so that educators and developers from many
backgrounds can share adapters safely. Inclusion is part of quality: a review
process that only works for people with spare time, fast hardware, or fluent
English produces a worse registry.

This document sets expectations for contributors, reviewers, and moderators.
It complements [moderation.md](moderation.md) and applies to pull requests,
issues, reviews, discussions, and any project space.

## Expectations

- **Be specific and kind.** Critique code and evidence, not the author. Explain
  why a change is needed and, when possible, link to a rule or example.
- **Assume good faith while verifying claims.** Ask questions before
  attributing motives. Security review still requires evidence.
- **Respect names and identities.** Use the names and pronouns people use.
  Do not deadname, misgender, or speculate about someone's background.
- **No harassment or discrimination.** This includes slurs, threats, sexual
  attention, exclusion based on race, ethnicity, nationality, religion, caste,
  disability, age, gender identity, sexual orientation, or socioeconomic
  status.
- **Protect privacy.** Do not post student work, grades, personal data,
  credentials, private keys, or someone's contact details. Redact logs and
  screenshots.
- **No retaliation.** Reporting a problem or appealing a decision must never
  lead to retaliation, including review delays or hostile comments.

## Language and communication

- The working language is English, but reviewers should be patient with
  grammar, spelling, and terminology. A language error is not a code defect.
- Contributors may ask for clarification, examples, or more time. Reviewers
  should paraphrase when a comment is ambiguous.
- Do not require a particular chat platform, time zone, or synchronous meeting.
  Decisions belong in the pull request so everyone can read them later.
- If you are comfortable doing so, help translate user-facing documentation.
  Translation help is a contribution, not an admission of weakness.

## Accessibility and contributor support

- Prefer plain-language README steps and copy-pasteable commands.
- Do not require proprietary tools, paid services, or high-end hardware to run
  the checks. The registry tools use Python and minisign only.
- Describe expected outputs and failure modes so contributors can debug
  without guessing.
- Offer a "good first review" path: a maintainer can explain a failing check
  before declining a pull request.
- Keep examples free of culturally specific assumptions and unnecessary
  personal data.
- When a contributor cannot run a check locally, a reviewer may reproduce it,
  but the contributor still owns the change.

## Review conduct

Reviewers should:

- state the blocking issue first, then optional suggestions;
- distinguish "must fix" from "nice to have";
- avoid scope creep into unrelated refactors;
- reply to questions even when the answer is "not in this change";
- recuse themselves when they have a conflict of interest.

Reviewers should not:

- demand private information or a video call;
- mock a contributor's code, language, or tooling choices;
- use an automated check as a substitute for explaining a decision;
- approve an adapter they do not understand.

## Reporting and enforcement

Report a conduct problem privately to the maintainers in `CODEOWNERS`. Include
what happened, where, and any supporting links; do not include student data or
secrets. If the report involves a maintainer, send it to another maintainer
listed there.

Moderators will:

1. acknowledge the report and avoid assigning anyone involved;
2. keep the report confidential where practical;
3. hear from the people involved before deciding, unless immediate action is
   needed to prevent harm;
4. choose a proportional response, such as a private clarification, a warning,
   a temporary pause on reviews, or removal from the project;
5. record the outcome privately and publish a summary when it is safe and useful.

Good-faith mistakes are handled with teaching and a chance to correct them.
Threats, doxxing, and repeated harassment may lead to immediate removal.

## Accessibility requests

If a project process is inaccessible to you (for example, the review format,
the tools, or the terminology), open an issue or tell a maintainer. We will try
to provide an alternative that preserves the same safety checks. The goal is a
registry that more people can contribute to without lowering the bar for
student safety.
