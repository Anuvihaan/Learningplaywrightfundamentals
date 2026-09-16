# Learning Playwright Fundamentals

A hands-on Playwright + TypeScript project used to learn test automation fundamentals.

## Prerequisites

- [Node.js](https://nodejs.org/) 18 or later (includes `npm`)
- A code editor such as [VS Code](https://code.visualstudio.com/)

Verify your setup:

```bash
node -v
npm -v
```

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/Anuvihaan/Learningplaywrightfundamentals.git
   cd Learningplaywrightfundamentals
   ```

2. Install the project dependencies:

   ```bash
   npm install
   ```

3. Install the Playwright browsers and OS dependencies:

   ```bash
   npx playwright install
   ```

   On Linux CI machines you may also need:

   ```bash
   npx playwright install --with-deps
   ```

## Basic Setup

The project layout is:

```
.
├── tests/                  # Test specs (*.spec.ts)
│   ├── example.spec.ts
│   └── tta.check.spec.ts
├── playwright.config.ts    # Playwright configuration
└── package.json
```

Key settings in `playwright.config.ts`:

| Setting          | Value            | Meaning                                   |
| ---------------- | ---------------- | ----------------------------------------- |
| `testDir`        | `./tests`        | Where Playwright looks for test files      |
| `fullyParallel`  | `true`           | Run tests in files in parallel             |
| `reporter`       | `html`           | Generates an HTML report                   |
| `trace`          | `on-first-retry` | Records a trace when a test is retried     |
| `headless`       | `false`          | Opens a visible browser window             |
| `projects`       | `chromium`       | Runs against Desktop Chrome                |

Set `headless: true` in `playwright.config.ts` to run without opening a browser window.

## Writing a Test

Create a file in `tests/` ending with `.spec.ts`:

```ts
import { test, expect } from '@playwright/test';

test('has title', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  await expect(page).toHaveTitle(/Playwright/);
});
```

## Running Tests

Run all tests:

```bash
npx playwright test
```

Run a single file:

```bash
npx playwright test tests/example.spec.ts
```

Run tests in headed mode:

```bash
npx playwright test --headed
```

Run a specific project (browser):

```bash
npx playwright test --project=chromium
```

Debug a test with the Playwright Inspector:

```bash
npx playwright test --debug
```

## Reports

After a run, open the HTML report:

```bash
npx playwright show-report
```

## Generating Tests

Record new tests with codegen:

```bash
npx playwright codegen https://playwright.dev/
```

## Useful Scripts

Add these to `package.json` to shorten the commands:

```json
"scripts": {
  "test": "playwright test",
  "test:headed": "playwright test --headed",
  "test:debug": "playwright test --debug",
  "report": "playwright show-report"
}
```

Then run them with `npm test`, `npm run test:headed`, and so on.
