# pnpm .bin cleanup

When using `node-linker=hoisted`, with workspaces, if package versions get out of sync and then back in sync, the `.bin` is not cleaned up.

In this example `package-a` has had it's vite package bumped to latest, while `package-b` has been left on an earlier version. This means you end up with a version of `vite` installed inside the package `node_modules` along with the necessary `.bin` files.

![image](https://github.com/donkeyote/pnpm-bin-cleanup/assets/25738591/1623baa6-e6d8-4f41-8d87-16fa534fb74a)

You realise your mistake, and then update the forgotten package to latest `vite` as well. After running `pnpm i` the `node_modules` are cleaned up as only one version of `vite` exists in the workspace, but the `.bin` files still exist which causes errors when running the build command.

![image](https://github.com/donkeyote/pnpm-bin-cleanup/assets/25738591/220e139a-808d-4e9a-8d9d-131d718ae528)


## Steps to recreate

NOTE: Seems to be windows only. I was unable to recreate on a mac.

- Checkout repo and run `pnpm i`
- Update `package-b` to `vite` version 5.0.12
- Run `pnpm i`
- Run build command in package-b
  - You should get a `Cannot find module` error
