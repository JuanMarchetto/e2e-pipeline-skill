---
name: e2e-pipeline
description: "Generate and run E2E tests for mobile and web apps. Discovers screens, states, and flows via static analysis, generates Maestro test flows with assertions, executes them, and reports pass/fail results. Use when: run e2e, test flows, e2e tests, generate tests, automated testing."
license: MIT
metadata:
  version: 1.0.0
  category: toolchain
  tags: [e2e, testing, maestro, expo, react-native, assertions, quality]
---

# E2E Pipeline

Generate and run comprehensive E2E tests from static analysis of your codebase. Discovers all screens, states, and flows, generates Maestro test flows with real assertions, and reports results.

**Pipeline:** DISCOVER > DETECT > GENERATE > EXECUTE > REPORT

## Quick Start

User says: "run e2e" or "test flows"

You:
1. Discover all screens, states, and flows via static analysis
2. Detect project type and running state
3. Generate test flows with assertions for each discovered flow
4. Present test plan for user approval
5. Execute all test flows
6. Report pass/fail results with failure screenshots

## Phase 0: DISCOVER -- Flow & State Analysis

Identical to demo-pipeline Phase 0. Analyze the codebase to map all screens, states, flows, and native dependencies.

### Step 1: Discover screens

Read the file-based routing structure:
```bash
find src/app -name "*.tsx" -not -name "_layout.tsx" -not -name "+not-found.tsx" | sort
```

### Step 2: Identify states per screen

Search for UI conditionals:
```bash
grep -n "loading\|isLogged\|isEmpty\|error\|balance.*>\|\.length.*==.*0" src/app/**/*.tsx
```

### Step 3: Map navigation flows

```bash
grep -rn "router\.push\|router\.replace\|href=" src/app/ src/components/
```

### Step 4: Identify native dependencies

```bash
grep -rn "expo-camera\|CameraView\|useMobileWallet\|SeedVault" src/
```

### Step 5: Present flow map

Present the discovered map and ask:
> "I discovered N screens with M states and K flows. Which flows do you want to test? (default: all)"

## Phase 1: DETECT -- Project Type & State

Same as demo-pipeline Phase 1. Detect project type (Expo/web) and check running state (device/emulator/server).

## Phase 2: GENERATE -- Create Test Flows

For each selected flow, generate a Maestro test YAML with assertions.

### Assertion mapping

| Unified | Maestro |
|---------|---------|
| `assert_visible: "Text"` | `- assertVisible: "Text"` |
| `assert_visible: { id: "elem" }` | `- assertVisible:\n    id: "elem"` |
| `assert_not_visible: "Text"` | `- assertNotVisible: "Text"` |
| `assert_contains: { id: "elem", text: "val" }` | `- assertVisible:\n    id: "elem"\n    text: "val"` |

### Test YAML format

```yaml
flow: "<Flow Name>"
steps:
  - name: "<Step Name>"
    actions:
      - tap: "Element"
      - assert_visible: "Expected Result"
      - screenshot_on_fail: true
```

### Present for approval

Show all generated tests to user:
> "I generated N test flows covering M screens. Review and approve, or select which to run."

**HARD GATE: Do NOT execute until user approves.**

## Phase 3: EXECUTE -- Run Tests

### Step 1: Generate Maestro test files

For each approved test flow, create a `.yaml` file in `./maestro/tests/`:

```bash
mkdir -p maestro/tests
```

### Step 2: Run all tests

```bash
maestro test maestro/tests/
```

Or run individually:
```bash
maestro test maestro/tests/flow-name.yaml
```

### Step 3: Capture failure screenshots

Maestro auto-captures on failure. Copy to project:
```bash
mkdir -p screenshots/failures
cp ~/.maestro/tests/*/screenshots/* screenshots/failures/ 2>/dev/null || true
```

## Phase 4: REPORT -- Results

### Step 1: Parse results

Collect pass/fail from Maestro output.

### Step 2: Generate report

Create `./docs/test-report.md` using [templates/test-report.md](templates/test-report.md):

```markdown
# E2E Test Report -- YYYY-MM-DD

## Summary
- Total: N flows
- Passed: X
- Failed: Y

| Flow | Steps | Result | Failure Screenshot |
|------|-------|--------|--------------------|
| ... | ... | Pass/Fail | path or N/A |
```

### Step 3: Present summary

> **E2E tests complete!**
> - Passed: X/N flows
> - Failed: Y (see screenshots in `./screenshots/failures/`)
> - Report: `./docs/test-report.md`

## Structure

```
e2e-pipeline/
├── SKILL.md           # This file
├── reference.md       # Complete action and assertion reference
└── templates/
    ├── maestro-test.yaml   # Maestro test template
    └── test-report.md      # Report template
```
