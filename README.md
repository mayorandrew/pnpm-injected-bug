# PNPM injected dependency update issue

This repository is a minimal reproduction of an issue with "injected" dependencies in PNPM 7.26.2.

It contains a `lib` and `app` which uses `lib` as a dependency.

To demonstrate the issue, perform the following steps on a freshly checked our repository:
1. Install deps with `pnpm i`
2. Build lib with `pnpm run -r build`. This creates `packages/lib/dist.js` file.
3. Observe that `packages/app/node_modules/lib` does not contain `dist.js` file. That is expected because `lib` is an "injected" dependency for `app`.
4. Run `pnpm i` to update `packages/app/node_modules/lib`.
5. Observe the output from step 4: 
   ```
   Scope: all 3 workspace projects
   Lockfile is up to date, resolution step is skipped
   Already up to date
   Done in 175ms
   ```
6. Observe that `packages/app/node_modules/lib` still does not contain `dist.js` file.

Expected behavior is that after step 4 `dist.js` should be present in `packages/app/node_modules/lib`.
