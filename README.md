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
[23:05:42.171] 
[23:05:42.171] devDependencies:
[23:05:42.171] + @sveltejs/adapter-auto 1.0.0-next.88
[23:05:42.171] + @sveltejs/adapter-vercel 1.0.0-next.81
[23:05:42.171] + @sveltejs/kit 1.0.0-next.551
[23:05:42.171] + svelte 3.53.1
[23:05:42.171] + svelte-check 2.9.2
[23:05:42.171] + svelte-preprocess 4.10.7
[23:05:42.171] + tslib 2.4.1
[23:05:42.171] + typescript 4.9.3
[23:05:42.171] + vite 3.2.4
[23:05:42.171] 
[23:05:42.172] Done in 3.3s
[23:05:42.193] Running "pnpm run build"
[23:05:42.607] 
[23:05:42.608] > vercel-split-test@0.0.1 build /vercel/path0
[23:05:42.608] > vite build
[23:05:42.608] 
[23:05:43.364] vite v3.2.4 building for production...
[23:05:43.386] transforming...
[23:05:43.850] ✓ 35 modules transformed.
[23:05:43.898] rendering chunks...
[23:05:43.939] vite v3.2.4 building SSR bundle for production...
[23:05:43.944] transforming...
[23:05:44.263] ✓ 54 modules transformed.
[23:05:44.285] Generated an empty chunk: "hooks"
[23:05:44.292] rendering chunks...
[23:05:44.307] .svelte-kit/output/server/vite-manifest.json                   1.39 KiB
[23:05:44.307] .svelte-kit/output/server/index.js                             96.18 KiB
[23:05:44.307] .svelte-kit/output/server/entries/fallbacks/layout.svelte.js   0.23 KiB
[23:05:44.308] .svelte-kit/output/server/entries/fallbacks/error.svelte.js    1.50 KiB
[23:05:44.308] .svelte-kit/output/server/entries/pages/_page.svelte.js        0.31 KiB
[23:05:44.308] .svelte-kit/output/server/chunks/index.js                      3.15 KiB
[23:05:44.308] .svelte-kit/output/server/chunks/hooks.js                      0.00 KiB
[23:05:44.555] 
[23:05:44.555] Run npm run preview to preview your production build locally.
[23:05:44.557] .svelte-kit/output/client/vite-manifest.json                                         2.78 KiB
[23:05:44.558] .svelte-kit/output/client/_app/immutable/components/layout.svelte-5d5c2819.js        0.53 KiB / gzip: 0.35 KiB
[23:05:44.558] .svelte-kit/output/client/_app/immutable/components/error.svelte-0f9e2f60.js         2.07 KiB / gzip: 0.95 KiB
[23:05:44.558] .svelte-kit/output/client/_app/immutable/components/pages/_page.svelte-aa58de35.js   0.81 KiB / gzip: 0.47 KiB
[23:05:44.558] .svelte-kit/output/client/_app/immutable/chunks/singletons-1aa68ebc.js               1.99 KiB / gzip: 1.07 KiB
[23:05:44.559] .svelte-kit/output/client/_app/immutable/chunks/0-000c53bf.js                        0.09 KiB / gzip: 0.09 KiB
[23:05:44.559] .svelte-kit/output/client/_app/immutable/chunks/1-632860ef.js                        0.09 KiB / gzip: 0.09 KiB
[23:05:44.559] .svelte-kit/output/client/_app/immutable/chunks/2-234451a5.js                        0.09 KiB / gzip: 0.10 KiB
[23:05:44.559] .svelte-kit/output/client/_app/immutable/chunks/index-4ce6debf.js                    6.71 KiB / gzip: 2.72 KiB
[23:05:44.559] .svelte-kit/output/client/_app/immutable/start-745d8c8c.js                           26.54 KiB / gzip: 10.21 KiB
[23:05:44.564] 
[23:05:44.564] > Using @sveltejs/adapter-vercel
[23:05:45.002]   ✔ done
[23:05:45.057] Build Completed in /vercel/output [6s]
[23:05:45.522] An unexpected error happened when running this build. We have been notified of the problem. If you have any questions, please contact support@vercel.com.
```

Using `"@sveltejs/adapter-vercel": "1.0.0-next.81"` instead of `next` produces the same error.

Changing `split: false` in svelte.config.js works ok


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
