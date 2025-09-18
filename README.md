# eslint-config-mckelveygreg

[![publish](https://github.com/mckelveygreg/eslint-config-mckelveygreg/actions/workflows/publish.yml/badge.svg)](https://github.com/mckelveygreg/eslint-config-mckelveygreg/actions/workflows/publish.yml)
![npm (scoped)](https://img.shields.io/npm/v/@mckelveygreg/eslint-config)

## Usage

### eslint.config.js

```js
import baseConfig from "@mckelveygreg/eslint-config"
import reactConfig from "@mckelveygreg/eslint-config/react"
import pluginQuery from "@tanstack/eslint-plugin-query"
import tsEslint from "typescript-eslint"

export default tsEslint.config(
  {
    // global ignores
    ignores: ["**/generated/**/*"],
  },
  {
    files: ["**/*.{js,jsx,ts,tsx}"],
    extends: [
      // mckelveygreg configs
      ...baseConfig,
      ...reactConfig,
      // other configs
      ...pluginQuery.configs["flat/recommended"],
    ],
    // for type aware linting
    languageOptions: {
      parserOptions: {
        projectService: true,
      },
    },
    // custom rules
    rules: {
      "react/prop-types": "off",
      "no-debugger": "warn",
    },
  },
  // scoped rules
  {
    files: ["./packages/app/src/**/*.{js,jsx,ts,tsx}"],
    rules: {
      "@typescript-eslint/switch-exhaustiveness-check": "error",
    },
  }
)
```

### prettier.config.js

We can share prettier configs now! 🎉

```js
import sharedConfig from "@mckelveygreg/eslint-config/prettier.config"
/**
 * @see https://prettier.io/docs/en/configuration.html
 * @type {import("prettier").Config}
 */
const config = {
  ...sharedConfig,
  // override any shared config here
  arrowParens: "avoid",
}

export default config
```

## Configure VS Code

[1]: https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint
[2]: https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode

1. Make sure these extensions are installed:
   - [ESlint][1]
   - [Prettier - Code formatter][2]
   - Install from the links above or _via terminal:_

   ```sh
   code --install-extension dbaeumer.vscode-eslint && \
   code --install-extension esbenp.prettier-vscode
   ```

2. Add the following to your global `~/.vscode/settings.json` file:

   ```jsonc
   {
     "editor.codeActionsOnSave": {
       "source.fixAll": true,
     },
     // format on save for everything, but what prettier will handle through eslint
     "editor.formatOnSave": true,
     "[javascriptreact]": {
       "editor.formatOnSave": false,
     },
     "[javascript]": {
       "editor.formatOnSave": false,
     },
     "[typescript]": {
       "editor.formatOnSave": false,
     },
     "[typescriptreact]": {
       "editor.formatOnSave": false,
     },
   }
   ```

3. Restart VS Code

## References

Inspired heavily by [eslint-config-wesbos](https://github.com/wesbos/eslint-config-wesbos)

---
