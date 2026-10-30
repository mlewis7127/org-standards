# org-standards

Engineering standards for every team, served to coding assistants over MCP.

Each file is one standard. Changes go through a pull request reviewed by the
standard's owner. On merge to `main`, a GitHub Action publishes the files, and every
assistant connected to the standards server sees the new version on its next
question. Nobody copies these files into their own repo.

## Layout

| Path | Standard | Owner |
|---|---|---|
| `compute/serverless.md` | Compute: serverless first | Platform Engineering |
| `data/relational.md` | Relational data stores | Architecture Guild |
| `data/key-value.md` | Key-value data stores | Architecture Guild |
| `integration/queue-consumers.md` | Queue consumers | Architecture Guild |
| `integration/rate-limited-downstream.md` | Calling a rate-limited system | Architecture Guild |
| `storage/object-storage.md` | Object storage and uploads | Platform Engineering |
| `security/baseline.md` | Security baseline | Security Engineering |
| `observability/baseline.md` | Observability baseline | Platform Engineering |

## Writing a standard

- Front matter: `id`, `title`, `domain`, `owner`, `version`, `status`, and
  `checklist_for` (the component types the design checklist applies to).
- Rules use **MUST** or **SHOULD**, each with a reason.
- Say when to use what, not only what is allowed.
- No project-specific facts: no system names, limits or targets from a particular
  product. Those belong in that product's requirements.
- Bump `version` on every change: minor for a clarification, major for a rule change.

## Using it

Ask your assistant, for example: "Using org-standards, what must the design of a
queue consumer cover?" Every answer ends with the standard, its version and the
commit it came from.
