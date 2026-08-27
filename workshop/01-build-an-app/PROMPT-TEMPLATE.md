# Prompt Template – starter prompt

Copy, paste your spec where indicated, run in Claude Code:

```
Build a web app according to the spec below.

TECH RULES (non-negotiable):
- ONE single file: index.html. All HTML, CSS and JavaScript in that file.
- Plain JavaScript, no frameworks, no build steps, no npm, no server.
- Data is saved in localStorage.
- The app must work fully when I double-click the file (file://).
- If a library is truly needed (e.g. maps or charts): load it via CDN
  with <script src>, and verify it works over file://.
- Clean, modern design. English UI.

Build ONLY the happy path in the MVP line. Nothing else.

SPEC:
[paste your completed spec here]

When done: tell me exactly where index.html is located.
```

## Iteration prompts (step 2 and onward)

Make small, scoped requests against what exists – not new grand orders:

```
Add [one nice-to-have from the spec] to the existing flow. Change nothing else.
```

```
[Describe exactly what's wrong – what you did and what happened.] Fix it.
```

```
Make the UI nicer: better typography, spacing and color. Same functionality.
```

## Tips

- One request per prompt. Wait, look, prompt again.
- Open index.html in the browser after every change (reload with Cmd/Ctrl+R).
- If Claude suggests npm, a server or a framework: point it back to the tech rules.
