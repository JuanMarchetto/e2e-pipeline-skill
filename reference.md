# E2E Pipeline — Action & Assertion Reference

## Test Actions

All actions from demo-pipeline are supported: `tap`, `type`, `scroll`, `swipe`, `wait`, `wait_for`, `screenshot`.

## Assertion Actions

### assert_visible

Assert an element is visible on screen.

```yaml
- assert_visible: "Welcome"
```
- Maestro: `- assertVisible: "Welcome"`

**By testID:**
```yaml
- assert_visible: { id: "home-balance" }
```
- Maestro: `- assertVisible: { id: "home-balance" }`

### assert_not_visible

Assert an element is NOT on screen.

```yaml
- assert_not_visible: "Error"
```
- Maestro: `- assertNotVisible: "Error"`

### assert_contains

Assert an element contains specific text.

```yaml
- assert_contains: { id: "balance-display", text: "5,000" }
```
- Maestro: `- assertVisible: { id: "balance-display", text: "5,000" }`

### screenshot_on_fail

Capture screenshot only if a previous assertion fails. Applied per step.

```yaml
- screenshot_on_fail: true
```

## Test YAML Structure

```yaml
flow: "<Flow Name>"
steps:
  - name: "<Step Name>"
    actions:
      - <action or assertion>
      - screenshot_on_fail: true    # optional
```
