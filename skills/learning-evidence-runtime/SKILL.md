---
name: learning-evidence-runtime
description: "Background contract reference for host code implementing or verifying NanoClaw learning evidence. Use for transcript-view integration, scanner dependency checks, or questions about the learning runtime manifest. Supplies context; performs no installation, audit or approval."
user-invocable: false
---

# Learning Evidence Runtime Reference

Process steps in order. Do not skip ahead.

## Step 1 — Read The Declaration

Read the installed host package's declaration:

```text
skills/learning-evidence-runtime/runtime.json
```

The named `rules/learning-evidence-runtime.md` authority owns the interpretation and consuming boundaries. A missing or unsupported declaration is not a compatible runtime. Proceed immediately to Step 2.

## Step 2 — Explain The Consuming Boundary

Use the declaration and authority to explain the requested host integration or verification. Identify which component supplies the dependency and which caller must enforce source selection and refusal handling. Keep deployment evidence separate from the declared interface: the manifest alone proves no running image, available mount or owner approval. Return the relevant contract and any missing evidence. Finish here.
