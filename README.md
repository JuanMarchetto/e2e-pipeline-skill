# E2E Pipeline

Generate and run E2E tests for mobile and web apps. Discovers all screens, states, and flows via static analysis, generates Maestro test flows with real assertions, executes them, and reports pass/fail results.

## Install

Via the marketplace:
```
/plugin marketplace add JuanMarchetto/agent-skills
/plugin install e2e-pipeline@agent-skills
```

Or via [skills.sh](https://skills.sh):
```bash
npx skills add JuanMarchetto/e2e-pipeline-skill
```

Or manually:
```bash
git clone https://github.com/JuanMarchetto/e2e-pipeline-skill.git
cp -r e2e-pipeline-skill ~/.claude/skills/e2e-pipeline
```

## How It Works

```
"run e2e" or "test flows"
            |
    Phase 0: DISCOVER
    Static analysis -> screens, states, flows
            |
    Phase 1: DETECT
    Project type + running state
            |
    Phase 2: GENERATE
    Maestro test YAMLs with assertions
            |
    Phase 3: EXECUTE (after user approval)
    Run all test flows
            |
    Phase 4: REPORT
    Pass/fail summary + failure screenshots
```

## What's Inside

```
e2e-pipeline-skill/
├── SKILL.md                    # Full pipeline instructions
├── reference.md                # Action & assertion reference
└── templates/
    ├── maestro-test.yaml       # Maestro test template
    └── test-report.md          # Report template
```

## Requirements

- Maestro CLI (`maestro`)
- Running app on device/emulator or web server

## License

[MIT](LICENSE)
