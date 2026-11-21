# Security policy

## Reporting a vulnerability

Report suspected signature bypasses, key compromise, malicious adapters, or
sandbox escape privately to the registry maintainers. Do not include private
keys, passwords, student data, or exploit details in a public issue.

Use GitHub's private vulnerability reporting for this repository when enabled,
or contact the maintainers listed in `CODEOWNERS`.

Include:

- affected registry URL and `registryId`;
- `sequence`, key id, and public-key fingerprint (never a secret);
- steps to reproduce;
- impact and any known exploitation.

We aim to acknowledge a report within 72 hours and to publish a coordinated
advisory after a fix or revocation is available.

## Signing keys

Each registry tier uses independent keys. Only public keys, key ids, and
fingerprints belong in this repository. This repository never contains an
official key.

| Tier | Key name | Key id | SHA-256 fingerprint | Owner |
| --- | --- | --- | --- | --- |
| community | `community-2026.pub` | `743C219E03A4F152` | `7a8c5f319caf0d0e7539558c56524e2a6c9385f615a8b242fb044c14ffffbedb` | Red Pen Community maintainers |
| community (next) | `community-2027.pub` | `BD8F6E2251F05236` | `88b0b3a071b797c9aae3d62e149afbc8e0529873126b831b16e83fd423df996b` | Red Pen Community maintainers |

The private keys, passwords, and offline backup locations stay in the private
operator runbook, never here. The community signing key is stored only in the
protected `registry-signing-community` environment.

Key ids are uppercase, byte-reversed 8-byte minisign ids; fingerprints are
the SHA-256 of the exact `.pub` file bytes. Both are derived from
`keys/<name>.pub`:

```sh
python scripts/key_id.py --public-key keys/<name>.pub --json
```

## Supported versions

Security fixes apply to the current signed registry sequence and the app
release that embeds the current and next public keys. Historical signed tags
are retained for audit but are not re-signed.

## Scope

In scope:

- the registry index, signature, workflows, scripts, schemas, and public keys;
- adapter code and manifests published by this registry;
- the signing environment configuration.

Out of scope:

- vulnerabilities in third-party adapters from other registries;
- GitHub platform vulnerabilities;
- denial of service against GitHub or the app's download host.

## No secrets

This repository must never contain a private key, password, token, student
file, or build output. If one is committed, treat it as compromised, rotate the
key, and rewrite the repository only after revocation.
