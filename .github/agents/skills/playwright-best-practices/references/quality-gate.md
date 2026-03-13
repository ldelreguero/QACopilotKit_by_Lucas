# Playwright Quality Gate

Use this file as the canonical quality gate for Playwright work in this repository.

When a local agent, skill, or prompt needs to enforce Playwright standards, it should reference this file instead of redefining the same checklist.

## Mandatory Standards

### Locator order

Use locators in this order of preference:

1. `getByRole`
2. `getByLabel` and `getByPlaceholder`
3. `getByText` and `getByTitle`
4. `getByTestId` when semantic locators are not enough
5. CSS or XPath only as a justified last resort

If multiple matches are expected, narrow with filters first and then use `.first()`, `.last()`, or `.nth()` deliberately.

### Waiting and timing

- Do not use `waitForTimeout` in production test code.
- Prefer Playwright auto-waiting, explicit expectations, and observable state transitions.
- Treat arbitrary sleeps as a debugging-only fallback.

### Page Object Model

- Keep reusable UI interactions in page objects or equivalent abstractions.
- Use clear class names such as `PageNamePage` when the repo follows a pages-based structure.
- Keep assertions in tests unless the abstraction explicitly represents a domain-level check.

### Assertions

Prefer these assertions when applicable:

- `toHaveText`
- `toContainText`
- `toHaveCount`
- `toHaveURL`

Use `toMatchAriaSnapshot()` when accessibility semantics are part of the expected outcome.
Use `toBeVisible()` only when visibility itself is the behavior being verified.

### Test readability

- Use `test.describe()` by feature or behavior.
- Use `beforeEach()` only for shared setup that improves readability.
- Use `test.step()` for longer flows with meaningful milestones.
- Name tests as `Feature - Action or Scenario`.

### File structure

When the repo uses a standard Playwright layout:

- keep test files under `tests/`
- keep page objects under `pages/`
- name files predictably, such as `feature.spec.ts` and `page-name.page.ts`

Local repo conventions may specialize the structure, but they must not contradict this gate.

## Canonical References

- Locator strategy: [locators.md](locators.md)
- Waiting and assertions: [assertions-waiting.md](assertions-waiting.md)
- Test structure and organization: [test-organization.md](test-organization.md)
- Page Object Model: [page-object-model.md](page-object-model.md)
- Debugging: [debugging.md](debugging.md)

## Rule of Precedence

If another active file in this repository repeats Playwright standards and conflicts with this gate, this file and the referenced Playwright base documents take precedence.
