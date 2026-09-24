# Model Artifact Metadata

This repository publishes JSON Schemas and publication rules for signed, versioned model artifact metadata.

No catalog, package manifest, model weights, signing key, or public trust root is published here yet.

## Contents

- `schemas/model-package.schema.json`: package metadata.
- `schemas/signed-model-package.schema.json`: package metadata and its Ed25519 signature.
- `schemas/signed-model-catalog.schema.json`: expiring catalog and its signature.
- `PUBLISHING.md`: release, signing, and repository controls.

Model binaries are hosted separately at immutable HTTPS object URLs. This repository must not contain application source, application-specific descriptions or identifiers, internal design material, logs, credentials, private keys, or unpublished test fixtures.
