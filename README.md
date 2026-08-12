# arc-release-digests

Cross-origin release digests for the [`arc` CLI](https://arc.afsd.io).

## What this is

`install.sh` downloads the `arc` tarball from `dl.arcblock.io` (a Cloudflare R2
bucket) and verifies it against the SHA-256 digests published **here** — a
deliberately separate origin.

A checksum served from the same origin as the download proves nothing about
substitution: whoever can serve a poisoned tarball can serve a matching checksum
next to it. Anchoring the digest in this repository means an attacker would have
to compromise both R2 **and** leave a matching, publicly visible commit in this
repository's history.

The same reasoning applies to the version pointer. `digests/latest` — not R2's
`manifest.json` — is what `install.sh` resolves an unpinned install against, so
a compromised R2 cannot silently roll you back to an old, vulnerable release.

## Layout

| Path | Contents |
| --- | --- |
| `digests/<version>.sha256` | One `<sha256>  <asset>` line per published platform |
| `digests/latest` | The current version string, e.g. `2.0.0-beta.34` |

## Do not hand-edit

Every commit here is written by the `publish-digests` job in `ArcBlock/arc`'s
release workflow. The commit history **is** the audit trail, so an unexplained
manual commit is indistinguishable from a compromise. If a digest ever has to be
published or corrected by hand, say why in the commit message.

Background: `ArcBlock/arc#3119`.
