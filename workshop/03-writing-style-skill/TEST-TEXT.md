# Test Text · Raw developer notes ahead of release 4.7

Use this as the source. The task for Claude: "Write customer-facing release notes from the notes below."

---

sprint 42-43 changes:

- SOK-4711: fixed the export bug, PDF export crashed if the case had >100 attachments, root cause was a timeout in the rendering service. thanks Mike for the debugging!!
- new feature: filtering in the case list!! you can now filter by admin and status at the same time. super smooth. toggled in settings by superadmin.
- SOK-4699: added validation on the national ID field in the registration flow, previously you could type literally anything lol
- performance: case list now loads ~40% faster due to new indexing (branch perf/list-index). users with big datasets will notice a big difference
- breaking: the old API endpoint /api/v1/cases is deprecated, gone in 5.0. customers who integrate must move to /api/v2/cases. docs on the docs site.
- SOK-4720: fixed typo in the login error message ("pasword")
- misc small fixes in the map component, incl. markers disappearing on zoom in iOS safari
- NOTE: release requires a db migration, maintenance window ~15 min, planned for wednesday night
