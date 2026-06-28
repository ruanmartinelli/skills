---
name: architect
description: "Plan before coding. Sketch the file tree, an ASCII diagram of the flow, and function signatures with a rough outline of what each does. Use for /architect, 'architect this', 'design this', or any non-trivial work where jumping to code would lock in the wrong shape."
disable-model-invocation: true
---

# Architect

Plan before coding. Produce a short design sketch, get it right, then implement against it. Don't jump to code for non-trivial work — it locks in the wrong shape.

## Output

Produce a plan with these sections. Skip any that don't apply.

### File tree
If new files or modules are needed, list them:

```
src/
├── auth/
│   ├── session.ts
│   └── tokens.ts
└── routes/
    └── login.ts
```

Note which files are new vs modified.

### Data model
If the change is shape-of-data driven (new tables, columns, or core types), sketch the schema and its relations:

```
User { id, email, planId→Plan }
Plan { id, tier, quotaMb }
```

### State machine
If behavior is mode/transition driven (auth flows, job lifecycle, retry logic), draw the states and transitions:

```
idle ──start──▶ running ──done──▶ done
                   │
                   └──fail──▶ failed ──retry──▶ running
```

### ASCII diagram
If there's a flow worth visualizing (request path, data pipeline), draw it:

```
client ──▶ login route ──▶ issueToken ──▶ setCookie
                              │
                              └──▶ audit.log()
```

Skip for changes with no meaningful flow.

### Function signatures
List every new/changed function with its signature and a one-line outline of what it does. No bodies.

```ts
// issueToken(userId: UserId): Token
//   mints a signed session token for the user

// setCookie(res, token): void
//   writes the session cookie on the response
```

## Process

1. **Ground.** Read the modules the change touches. Note constraints.
2. **Sketch.** Write the plan above. If the shape feels wrong, redo it before coding — cheap here, expensive later.
3. **Implement.** Fill in bodies against the plan. If a function needs something the plan didn't anticipate, surface it; don't bolt it on silently.
4. **Scrap.** If implementation keeps hitting friction of the same shape, the plan is wrong. Throw it out and re-sketch.
