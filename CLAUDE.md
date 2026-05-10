# ike-example-its

Integration test harness for the IKE Network release cascade. Pluggable
into `ike-example-ws` as an optional subproject (#343), but also runnable
standalone.

## First Steps

```bash
mvn validate    # unpack build standards into .claude/standards/
mvn verify      # run all IT cases
```

If `mvn validate` fails because `ike-build-standards` isn't in the local
repository, install it first via `ike-tooling`'s reactor.

## Build

```bash
mvn verify                       # all IT cases
mvn verify -Dinvoker.test=NAME   # one case
```

## Key Conventions

- Maven 4 with POM modelVersion 4.1.0
- Parent: `network.ike.platform:ike-parent` (from ike-platform) — gives
  this harness Java 25 build conventions, distributionManagement, and the
  same plugin matrix as the workspace it slots into.
- IT-target version pins live under `it.*` properties (`it.ike-tooling.version`,
  `it.ike-docs.version`, `it.ike-platform.version`) so they don't shadow the
  inherited ike-parent properties that control build-time plugin resolution.
  The `<filterProperties>` block renames them back to the unqualified form
  for substitution into IT-case POMs.

## Prohibited Patterns

- **Never use `maven-antrun-plugin`** — use a proper Maven goal
- **Never use `build-helper-maven-plugin` for multi-execution property
  chaining** — write a proper Maven goal in `ike-maven-plugin`
- **Never embed shell commands inline in POM** — extract to a named script

## See also

- `README.adoc` for the planned IT inventory and the
  "would-have-been-caught" list.
- `IKE-Network/ike-issues#343` — the issue that motivated splitting this
  out of `ike-example-ws`.
