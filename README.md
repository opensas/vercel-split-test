# vercel-split-test

demo repo to reproduce the issue deploying to vercel with adeptar-vercel and `split: true` option

## Steps to reproduce

```
$ pnpm create svelte@latest vercel-split-test

✔ Which Svelte app template? › Skeleton project
✔ Add type checking with TypeScript? › Yes, using TypeScript syntax
✔ Add ESLint for code linting? No
✔ Add Prettier for code formatting? No
✔ Add Playwright for browser testing? No
```

create repo at github

```
cd vercel-split-test
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:opensas/vercel-split-test.git
git push -u origin main
```

Import project to vercel with default options

Deploy is ok!

## Add adapter vercel

Add "@sveltejs/adapter-vercel": "next" to the devDependencies in your package.json and run npm install

update svelte.config.js like this:

```js
import adapter from '@sveltejs/adapter-vercel';
import preprocess from 'svelte-preprocess';

/** @type {import('@sveltejs/kit').Config} */
const config = {
	// Consult https://github.com/sveltejs/svelte-preprocess
	// for more information about preprocessors
	preprocess: preprocess(),

	kit: {
		adapter: adapter({
			split: true
		})
	}
};

export default config;
```

Commit and push changes

Vercel deployment fails:

```
[23:05:37.074] Cloning github.com/opensas/vercel-split-test (Branch: main, Commit: 85e0833)
[...]

devDependencies:
+ @sveltejs/adapter-auto 1.0.0-next.88
+ @sveltejs/adapter-vercel 1.0.0-next.81
+ @sveltejs/kit 1.0.0-next.551
+ svelte 3.53.1
+ svelte-check 2.9.2
+ svelte-preprocess 4.10.7
+ tslib 2.4.1
+ typescript 4.9.3
+ vite 3.2.4

Done in 3.3s
Running "pnpm run build"

> vercel-split-test@0.0.1 build /vercel/path0
> vite build

vite v3.2.4 building for production...
transforming...
✓ 35 modules transformed.
rendering chunks...
vite v3.2.4 building SSR bundle for production...
transforming...
✓ 54 modules transformed.
Generated an empty chunk: "hooks"
rendering chunks...
.svelte-kit/output/server/vite-manifest.json                   1.39 KiB
.svelte-kit/output/server/index.js                             96.18 KiB
.svelte-kit/output/server/entries/fallbacks/layout.svelte.js   0.23 KiB
.svelte-kit/output/server/entries/fallbacks/error.svelte.js    1.50 KiB
.svelte-kit/output/server/entries/pages/_page.svelte.js        0.31 KiB
.svelte-kit/output/server/chunks/index.js                      3.15 KiB
.svelte-kit/output/server/chunks/hooks.js                      0.00 KiB

Run npm run preview to preview your production build locally.
.svelte-kit/output/client/vite-manifest.json                                         2.78 KiB
.svelte-kit/output/client/_app/immutable/components/layout.svelte-5d5c2819.js        0.53 KiB / gzip: 0.35 KiB
.svelte-kit/output/client/_app/immutable/components/error.svelte-0f9e2f60.js         2.07 KiB / gzip: 0.95 KiB
.svelte-kit/output/client/_app/immutable/components/pages/_page.svelte-aa58de35.js   0.81 KiB / gzip: 0.47 KiB
.svelte-kit/output/client/_app/immutable/chunks/singletons-1aa68ebc.js               1.99 KiB / gzip: 1.07 KiB
.svelte-kit/output/client/_app/immutable/chunks/0-000c53bf.js                        0.09 KiB / gzip: 0.09 KiB
.svelte-kit/output/client/_app/immutable/chunks/1-632860ef.js                        0.09 KiB / gzip: 0.09 KiB
.svelte-kit/output/client/_app/immutable/chunks/2-234451a5.js                        0.09 KiB / gzip: 0.10 KiB
.svelte-kit/output/client/_app/immutable/chunks/index-4ce6debf.js                    6.71 KiB / gzip: 2.72 KiB
.svelte-kit/output/client/_app/immutable/start-745d8c8c.js                           26.54 KiB / gzip: 10.21 KiB

> Using @sveltejs/adapter-vercel
  ✔ done
Build Completed in /vercel/output [6s]
An unexpected error happened when running this build. We have been notified of the problem. If you have any questions, please contact support@vercel.com.
```

Using `"@sveltejs/adapter-vercel": "1.0.0-next.81"` instead of `next` produces the same error.

Changing `split: false` in svelte.config.js works ok

```
[...]
.svelte-kit/output/server/chunks/index.js                      3.15 KiB
.svelte-kit/output/server/chunks/hooks.js                      0.00 KiB
  ✔ done
Build Completed in /vercel/output [10s]
Generated build outputs:
 - Static files: 12
 - Serverless Functions: 1
 - Edge Functions: 0
Serverless regions: Washington, D.C., USA
Deployed outputs in 2s
Build completed. Populating build cache...
Uploading build cache [12.14 MB]...
Since the Node.js version () or package manager  could not be determined, Build Cache will not be stored
Done with "."
```


## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npm create svelte@latest

# create a new project in my-app
npm create svelte@latest my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.
