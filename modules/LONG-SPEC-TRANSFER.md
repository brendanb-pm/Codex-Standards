# Long Specification Transfer Module

Load only when requirements/specifications are too large for a reliable single transfer or when explicit prompt-integrity assembly is required.

Use unique sequential chunk markers:

```text
<WORK>-01-BEGIN
...
<WORK>-01-END
```

Requirements:
- number chunks sequentially;
- use unique matching BEGIN/END markers;
- require completeness validation before execution;
- stop if any chunk is missing, duplicated, malformed, out of order, or incomplete;
- use a final integrity marker for important assembled specifications.

After assembly verify chunk count/order, matching markers, final integrity marker, target file existence, and expected content presence.

Do not execute against an incomplete transfer or infer missing requirements.
