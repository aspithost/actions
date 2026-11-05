# Copilot Instructions

## Commits

- Follow Conventional Commits, as the `@abelspithost/commitlint` preset enforces.
- Write the description in imperative mood, lowercase, without a trailing period: `fix: correct null check in parser`.
- Mark breaking changes with `!` or a `BREAKING CHANGE:` footer.
- Never edit `CHANGELOG.md` by hand. The changelog preset generates it.

## Style

Apply these rules to comments, JSDoc, README content, and commit bodies.

- Write in active voice. Name the actor: "The parser rejects empty input", not "Empty input is rejected".
- Avoid forms of the verb "to be" (is, are, was, were, be, been, being, am). Use a stronger verb.
- Keep sentences short and direct.
