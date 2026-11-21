# Red Pen community adapter registry

The community-run registry for Red Pen adapters. It publishes a schema 7 signed
index (`registry.json`) for manifest schema 5 / API 2, authenticates every
adapter file by SHA-256, and is signed by a separate community key in a
protected GitHub environment.

This registry is not the official registry. It is the `community` trust tier:
the app embeds the community public key, but community adapters are off by
default and require explicit user consent.

## What is in the box

```
registry.json                 Signed index (schema 7, API 2)
registry.config.json          Registry identity, tier, signing settings
example/                      A minimal working starter adapter
schemas/                      Registry/manifest schemas, mirror list, source marker
lua/red-pen-api.lua           Copied runtime API reference
scripts/registry.py           Canonical JSON, key ids, CI config helpers
scripts/update.py             Regenerate registry.json from adapter folders
scripts/validate.py           Schema, hash, runtime, and signature validation
scripts/sign.py               Local signing and test-key dry run
scripts/verify.py             Exact-byte signature verification
scripts/key_id.py             Print a public key id and fingerprint
tests/                        Unit and regression tests
docs/                         Format, signing, publishing, rotation, security,
                              moderation, and inclusion
.github/workflows/            validate, sign, sign dry run, release
.github/actions/              Pinned minisign installer
keys/                         Community public keys only (no secret material)
```

## Quick start

```sh
python -m pip install jsonschema
python scripts/update.py
python scripts/validate.py
python tests/run.py
```

With minisign installed, exercise the encrypted-key path without any production
secret:

```sh
python scripts/sign.py --test-key
```

## Registry format

`registry.json` is schema 7 and targets manifest schema 5 / API 2. Its exact
bytes are signed with a detached minisign signature
(`registry.json.minisig`). The index authenticates every adapter file by
SHA-256, so the app never trusts a manifest or script from an unsigned source.

The signed document carries:

- `schema: 7`, `kind: "red-pen-adapters"`;
- the reserved slug `registryId: "red-pen-community"`;
- a string `publisher` (`Red Pen Community`), `sequence`, `issuedAt`, and
  `expiresAt`;
- one entry per adapter with `apiVersion: 2`, `folder`,
  `files[{path, sha256}]`, and `readmeUrl`;
- mirrored `settings`, `keyInput`, `questionInput`, `output`, `runtimes`,
  `bootstrap`, grants, and picker fields from the manifest.

There is no root `name`; v2 uses `publisher`. Bare-string file entries, legacy
permissions, and manifest `language`/`miseTool` hints are rejected. A managed
runtime's primary tool is `manifest.runtimes[0].tool`.

See [docs/format.md](docs/format.md) for every field and the deterministic JSON
rules.

## Contributing and moderation

- Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.
- Every adapter is reviewed by a maintainer for safety, least privilege, and
  correctness. See [docs/moderation.md](docs/moderation.md).
- We welcome adapters from every background and language community. See
  [docs/inclusion.md](docs/inclusion.md).
- Only maintainers may change `schemas/`, `scripts/`, `keys/`, `CODEOWNERS`, or
  `.github/workflows/`.

## Signing

- Exact bytes, no JCS. The file is written with sorted keys, two-space indent,
  LF, and one trailing newline.
- minisign 0.12 with a password-encrypted key. Production keys are never
  unencrypted (`-W` is not used).
- The community registry uses `keys/community-2026.pub` in the protected
  environment `registry-signing-community`. It never uses an official key.
- The signing job checks out `eslam-allam/red-pen-registry-signing` at the
  commit SHA in `registry.config.json`. It never executes signing code from the
  commit being signed.
- Pull requests never receive signing secrets. A separate workflow runs a
  throwaway encrypted test key on every PR.
- The signer tags `registry-v<sequence>` and uploads `registry.json`,
  `registry.json.minisig`, `registry.sha256`, and `release.json`.

See [docs/signing.md](docs/signing.md) and [docs/key-rotation.md](docs/key-rotation.md).

## Trust tiers

| Tier | Meaning |
| --- | --- |
| `official` | Operated by the Red Pen project; signed key embedded in the app. |
| `community` | Community-run; signed by a separate key embedded in the app, off by default. |
| `third-party` | At your own risk; unsigned or signed with a key the app does not know, after explicit consent. |

The tier in `registry.config.json` is for CI and documentation. The app derives
trust from the registry URL and the embedded public key, never from a
self-asserted field.

## Publishing

1. Add or update an adapter folder following [CONTRIBUTING.md](CONTRIBUTING.md).
2. Run `python scripts/update.py` and `python scripts/validate.py`.
3. Open a pull request. `Validate registry` and `Sign dry run` must pass.
4. A maintainer reviews the adapter (see
   [docs/moderation.md](docs/moderation.md)) and merges it.
5. The signing workflow enters `registry-signing-community`, signs the merged
   bytes, tags `registry-v<sequence>`, and publishes the release.

See [docs/publishing.md](docs/publishing.md) for the full operator checklist.

## Security

Report suspected signature bypasses, key compromise, or malicious adapters
privately. See [SECURITY.md](SECURITY.md). Never commit a private key,
password, token, or student data.

## License

MIT. See [LICENSE](LICENSE).
