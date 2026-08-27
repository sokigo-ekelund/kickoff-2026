# Brief · The Inspector's Core Flow (excerpt from the "Playground Inspection" PRD)

## Background (30 seconds)

Under SS-EN 1176-7, municipalities must carry out documented playground inspections: routine visual inspection (weekly), operational inspection (every 1–3 months) and an annual main inspection. The inspector is in-house staff working in the field, on a phone, often wearing gloves and in bad weather.

## Roles

- **Inspector** – performs inspections on assigned playgrounds. This is whose flow you are designing.
- Deviations are graded in three risk classes: **A – accident risk, immediate action/closure**, **B – fix as soon as possible**, **C – remark, fix during planned maintenance**.

## The flow to prototype (mobile)

1. **My inspections.** List of assigned playgrounds sorted by due date. Overdue clearly marked.
2. **Start inspection.** Inspection type suggested automatically based on what is due (visual/operational). Inspector confirms.
3. **Checklist, item by item.** Each item is answered: **OK / Deviation / Not applicable**. Large touch targets – one hand must be enough.
4. **On Deviation:** mandatory description, risk class A/B/C, photo via the phone camera, optional link to a specific piece of equipment (swing, slide …).
5. **Open deviations from earlier inspections** are shown during the round and can be signed off as fixed (action description + photo) or left open.
6. **Finish:** summary of the round, sign-off (name + timestamp). The inspection is locked.
7. **Risk class A** triggers an immediate warning in the UI and the option to mark the equipment as closed.

## Example checklist items (visual inspection)

Vandalism · Broken/missing parts · Protruding screws/nails · Glass/litter · Surface condition (e.g. sand depth) · Fences and gates · Visible entrapment risks

## Non-functional requirements that shape the design

- Works in a mobile browser (Safari/Chrome), responsive.
- Photos taken with the camera input directly in the browser.
- Completed inspections are immutable – the sign-off should feel like a real closure.
