# Qiyu engineering documentation

This repository is the canonical home for cross-repository architecture, engineering guides, Wiki and Blog pages. Keep kernel/runtime/rootfs implementation in their owning code repositories. Kernel-specific build/rebase instructions may remain beside kernel source; project-wide planning belongs here.

Use Wiki for maintained guidance and Blog for dated engineering records. Label plans, offline builds, device results and physical acceptance separately. Preserve source references and checksums. Do not publish credentials, private device configuration or unreviewed build output. Keep links and Pages navigation usable.

The repository split is described in wiki/architecture.md. Kernel development remains in bitsk/qiyu-kernel with subsystem commits on actual upstream history. Keep required source inputs, component locks and asset hashes. Do not recreate provenance directories in the code or asset repositories; existing validated baselines and published releases remain unchanged.

Preserve original Git author and committer names, real email addresses, and actual commit dates. New commits use the configured contributor identity and real dates; do not anonymize Git metadata or set it to the Unix epoch. Keep activity dates, personal contacts, local paths, internal network addresses, device identifiers and calibration out of public code, documentation and release manifests. Preserve upstream history and license notices. Use version labels in release names, file names and Blog URLs. Reproducible build archives may use SOURCE_DATE_EPOCH=0 and source-prefix remapping.
