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

## Test Codegen

Codegen opens a browser and a Playwright Inspector window, watches what you do, and writes the
equivalent Playwright code for you. It is the fastest way to discover good locators.

Record a new test against a site:

```bash
npx playwright codegen https://playwright.dev/
```

Copy the generated code into a new file under `tests/`, for example `tests/codegen.spec.ts`:

```ts
import { test, expect } from '@playwright/test';

test('test', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  await page.getByRole('link', { name: 'Get started' }).click();
  await expect(page.getByRole('heading', { name: 'Installation' })).toBeVisible();
});
```

### Recording Assertions

In the Inspector, click the assertion icons (`assert visibility`, `assert text`, `assert value`)
to add `expect(...)` lines as you record. Without this, codegen only captures actions.

### Common Options

```bash
# Record against a specific browser
npx playwright codegen --browser firefox

# Emulate a mobile device
npx playwright codegen --device "iPhone 13"

# Set the window size
npx playwright codegen --viewport-size "1280,720"

# Write the generated test straight to a file
npx playwright codegen -o tests/codegen.spec.ts https://playwright.dev/

# Emulate a colour scheme
npx playwright codegen --color-scheme dark https://playwright.dev/

# Use a custom attribute (instead of data-testid) for test ID selectors
npx playwright codegen --test-id-attribute data-qa https://playwright.dev/

# Start from a saved login state so you skip the login steps
npx playwright codegen --load-storage=auth.json https://example.com
```

### Saving and Reusing Login State

Log in once, save the storage state, then reuse it in later recordings:

```bash
# Save cookies + localStorage after logging in
npx playwright codegen --save-storage=auth.json https://app.thetestingacademy.com/

# Reuse it in a later codegen session
npx playwright codegen --load-storage=auth.json https://app.thetestingacademy.com/
```

### Choosing the Output Language

By default codegen emits `playwright-test` syntax, which drops straight into this project.
Use `--target` to generate a different language:

```bash
npx playwright codegen --target=javascript https://playwright.dev/
```

Supported values include `javascript`, `playwright-test`, `python`, `python-async`,
`python-pytest`, `csharp`, `csharp-mstest`, `csharp-nunit`, `csharp-xunit`, `java`, and
`java-junit`. For this repository, leave the default `playwright-test` in place.

### Picking Locators

The Inspector's **Pick locator** button lets you hover over any element and see the locator
Playwright recommends for it. Copy that locator directly into your specs — it is a quicker
path than recording an action and deleting the extra lines.

## Useful Scripts

Add these to `package.json` to shorten the commands:

```json
"scripts": {
  "test": "playwright test",
  "test:headed": "playwright test --headed",
  "test:debug": "playwright test --debug",
  "codegen": "playwright codegen",
  "report": "playwright show-report"
}
```

Then run them with `npm test`, `npm run test:headed`, `npm run codegen https://playwright.dev/`,
and so on.
