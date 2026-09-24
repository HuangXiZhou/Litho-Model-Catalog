# Publication Rules

These rules apply to every catalog and package manifest published here.

## Content boundary

- Publish only schemas, signed catalog JSON, signed package-manifest JSON, and required licensing or attribution material.
- Do not publish application source, application-specific descriptions or identifiers, internal architecture, build configuration, customer or evaluation data, logs, test fixtures, or credentials.
- Do not commit private keys, seed phrases, access tokens, private trust-store material, or signing-machine configuration. A public signature and non-secret key identifier are allowed.
- Do not create placeholder packages, fabricated signatures, or entries that point to unreleased artifacts.
- Keep model binaries outside this Git repository. Use immutable HTTPS objects for artifacts.

## Admission before publication

Before a package is listed, retain evidence that the exact source revision, conversion process, and redistribution rights were reviewed for the intended distribution. Include required licenses, attribution, notices, a software bill of materials, and conversion notes as separately hashed files. Pin source and package revisions using 40-character lowercase hexadecimal identifiers. Compute each byte count and SHA-256 digest from the final object.

URLs must use HTTPS without embedded credentials. Each file may have one primary URL and at most seven mirrors; mirrors must serve byte-identical content. Never overwrite a published revision path. Package paths must be safe relative paths and may refer only to supported data files; archives, executables, scripts, and dynamic plug-ins are not package content. The schema limits each file to 128 GiB and each catalog to 500 entries. Additional semantic validation must reject duplicate file paths (case-insensitive), duplicate package IDs, duplicate URLs within a file, and a package whose aggregate file size exceeds 128 GiB. Every package must contain at least one weights, license, notice, SBOM, and conversion-notes file. A catalog must have a positive sequence, be issued no more than five minutes in the future, expire after issuance and within 31 days, and keep every publication timestamp within five minutes of the verifier clock.

Reject publication if evidence is missing. A metadata entry alone does not establish that an artifact is legal, compatible, or ready for use.

## Signing

- Use Ed25519. Keep private keys offline in a dedicated signing environment; never store them in this repository, source control, CI variables, or issue attachments.
- Use a regular private-key file with owner-only read/write permissions. Verify the signing key against the independently provisioned public trust root before signing.
- Sign each package manifest first. Its signature covers the manifest object; outer `keyID` and `signature` fields are excluded.
- Then sign the catalog. Its signature covers `schemaVersion`, `sequence`, `issuedAt`, `expiresAt`, and `packages`; outer `keyID` and `signature` fields are excluded.
- The signing payload is compact UTF-8 JSON with recursively sorted object keys and unescaped forward slashes. Catalog dates use ISO 8601 UTC at whole-second precision. Array order is preserved. Do not edit or reserialize signed content after signing.
- Increase the positive catalog sequence for every publication. Expiration must be later than issuance and no more than 31 days later. Never reuse a sequence for different catalog bytes.
- Verify all signatures and schemas before publication. Keep private signing keys separate from the public repository and rotate them through an independently authenticated trust-root update.

## Review and repository controls

Publish through a pull request and review the complete diff, including URLs, digests, revisions, signatures, and licensing files. Protect the default branch from force pushes and deletion. Keep issues, wiki, and projects disabled unless a clear operational need is approved.

Do not add CI workflows that receive signing secrets. Schema validation may run without credentials; signing remains an offline release step. After merge, fetch public files from the published endpoint and independently verify their hashes and signatures. Treat malformed, expired, replayed, or unverifiable metadata as unavailable; never fall back to unsigned metadata.
