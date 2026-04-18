create `scripts/clean.sh` `package.json` `Cargo.toml` `.gitignore`

`bun create vite@latest`: `ui-desktop` `ui-mobile`

`bun i`

`bun add -D @tauri-apps/cli@latest`

`bun tauri init --frontend-dist "../ui-desktop/dist" --dev-url "http://localhost:25000" --before-dev-command "bun -F ui-desktop dev" --before-build-command "bun -F ui-desktop build"`

modify `tauri.conf.json` identifier

`bun tauri android init`

create `src-tauri/tauri.mobile.conf.json`

modify `ui-desktop/vite.config.ts` `ui-mobile/vite.config.ts`

`bun tauri dev`

`bun tauri android dev -c src-tauri/tauri.mobile.conf.json`

> package.json

```json
{
  "name": "template-tauri-bun-workspace",
  "private": true,
  "version": "0.0.1",
  "scripts": {
    "tauri": "tauri",
    "clean": "sh ./scripts/clean.sh"
  },
  "workspaces": {
    "packages": ["ui-desktop", "ui-mobile"]
  }
}
```

> Cargo.toml

```toml
[workspace]
members = ["src-tauri"]
resolver = "2"
```

> .gitignore

```
node_modules
/target/
```

> tauri.mobile.conf.json

```json
{
  "$schema": "../node_modules/@tauri-apps/cli/config.schema.json",
  "productName": "template-tauri-bun-workspace",
  "version": "0.0.1",
  "identifier": "com.template-tauri-bun-workspace",
  "build": {
    "frontendDist": "../ui-mobile/dist",
    "devUrl": "http://localhost:26000",
    "beforeDevCommand": "bun -F ui-mobile dev",
    "beforeBuildCommand": "bun -F ui-mobile build"
  }
}
```

> vite.config.ts

```ts
import { defineConfig } from 'vite';
import react, { reactCompilerPreset } from '@vitejs/plugin-react';
import babel from '@rolldown/plugin-babel';

const host = process.env.TAURI_DEV_HOST;

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), babel({ presets: [reactCompilerPreset()] })],
  // Vite options tailored for Tauri development and only applied in `tauri dev` or `tauri build`
  //
  // 1. prevent Vite from obscuring rust errors
  clearScreen: false,
  // 2. tauri expects a fixed port, fail if that port is not available
  server: {
    port: 25000,
    strictPort: true,
    host: host || false,
    hmr: host
      ? {
          protocol: 'ws',
          host,
          port: 25001,
        }
      : undefined,
  },
});
```
