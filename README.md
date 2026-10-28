# Ticket #4471: support knowledge base

The original export is in `support-kb/`. The new page is `support-kb/src/routes/kb/incidents/4471-split/+page.svx`, with counts computed by `support-kb/src/lib/kb/ReportSplit.svelte`.

`4471-split.html` is a static, manually prepared reading view of that page for the 4 October export. It is not an actual Svelte render: the export has no application or build configuration. The signed desk reference is calculated using the export's `desk-ref.ts` algorithm. Counts are a snapshot and must be checked against the live desk.
