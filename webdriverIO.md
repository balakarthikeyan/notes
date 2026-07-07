## WebdriverIO

- Framework: WebdriverIO (v8+ recommended)
- Language: TypeScript
- Test Runner: Mocha
- Assertion Library: Chai
- Comparison Strategy: Run the same test suite against two base URLs (prod & dev) and compare results.

```bash
project-root
├── package.json
├── tsconfig.json
├── wdio.conf.ts
└── tests
    └── smoke.spec.ts
```

```bash
npm install --save-dev webdriverio @wdio/cli @wdio/local-runner @wdio/mocha-framework ts-node typescript chai
npm install --save-dev wdio-image-comparison-service
npx wdio config --spec ./tests/content.spec.ts
npx wdio run wdio.conf.ts
npx wdio run wdio.conf.ts --spec ./tests/content.spec.ts
npm config set strict-ssl false
npm install --global --production windows-build-tools
```

🔑 Key Points
- `scripts.test` → runs your WebdriverIO suite with wdio.conf.ts.
- `webdriverio + @wdio/* packages` → core framework.
- `typescript + ts-node` → TypeScript support.
- `chai` → assertions.
- `wdio-image-comparison-service` → visual regression.
- `dotenv` → load environment variables for prod/dev URLs.
- `@wdio/allure-reporter` → add more reporters.
- `@wdio/globals` → provides WebdriverIO globals like browser, $, $$ without needing imports.
- `strict:` Ensures strong typing, catches errors early.
- `outDir:` Keeps compiled JS separate from your source.
- `skipLibCheck:` Speeds up compilation by ignoring type issues in external libraries.

If you want to compile TypeScript separately: `npx tsc`

```bash
npm install --save-dev @wdio/allure-reporter allure-commandline
```
- `@wdio/allure-reporter` → integrates Allure with WebdriverIO.
- `allure-commandline` → lets you generate and serve reports from the CLI.

After running tests, generate the report:

```bash
npx allure generate allure-results --clean -o allure-report
npx allure open allure-report
```