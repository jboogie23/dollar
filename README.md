Dollar Holler Tutorial from Everything Svelte course

---

# Process to instal Svelte, TailwindCSS, and prettier-plugin-svelte (for auto ordering of classes)

## Svelte

Install per latest official docs using `pnpm`

## TailwindCC

Install per latest official docs using `pnpm`

## Prettier-plugin-svelte

Official site: https://github.com/sveltejs/prettier-plugin-svelte with modified instructions below:

1. run `pnpm install -D prettier prettier-plugin-tailwindcss` which will install the plugin into `node_modules` and add it into `package.json` dependencies.
2. updated `package.json` to: include `"prettier-plugin-svelte": "^3.0.3", "prettier-plugin-tailwindcss": "0.0.0-insiders.ac70a66",`.
3. run `pnpm i` to update to the packages defined in the updated `package.json`.
4. see if fix tailwind is autosorting, otherwise restart vscode

NB: There is no need to change `.prettierrc` to `prettier.config.cjs` and change the formatting. There is no need to change .js to .cjs.
