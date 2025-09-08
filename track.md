```
pnpm create tauri-app
```

```
$ pnpm create tauri-app
.../1992a354882-3bc4                     |   +2 +
.../1992a354882-3bc4                     | Progress: resolved 12, reused 2, downloaded 0, added 2, done
✔ Project name · template-tauri-restructure
✔ Identifier · com.template-tauri-restructure
✔ Choose which language to use for your frontend · TypeScript / JavaScript - (pnpm, yarn, npm, deno, bun)
✔ Choose your package manager · pnpm
✔ Choose your UI template · React - (https://react.dev/)
✔ Choose your UI flavor · TypeScript

Template created! To get started run:
  cd template-tauri-restructure
  pnpm install
  pnpm tauri android init

For Desktop development, run:
  pnpm tauri dev

For Android development, run:
  pnpm tauri android dev


```

`src` rename `src-react`

`index.html` move to `src-react/index.html`

modify `vite.config.ts`

```
build.outDir '../dist'
root         'src-react'
publicDir    '../public'
```

modify `src-react/index.html`

```
/src/ -> /
```

modify `tsconfig.json`

```
"include": ["src"] -> "include": ["src-react"]
```

add `Cargo.toml`

```
[workspace]
members = ["src-tauri"]
resolver = "2"
```

将 `src-tauri/.gitignore` 内容复制到 `.gitignore` 后，删除 `src-tauri/.gitignore`

run command

```
pnpm i
pnpm dev
pnpm tauri dev
```

调整 `src-tauri/Cargo.toml` 版本号到最新，解决下面的报错：

```
> tauri "dev"

     Running BeforeDevCommand (`pnpm dev`)
       Error Failed to parse version `2` for crate `tauri`
       Error Failed to parse version `2` for crate `tauri-plugin-opener`
```

run command

```
pnpm tauri android init
```

modify `.gitignore`

```
/gen/schemas -> src-tauri/gen/schemas
```
