[![add-on registry](https://img.shields.io/badge/DDEV-Add--on_Registry-blue)](https://addons.ddev.com)
[![tests](https://github.com/tyler36/ddev-vite/actions/workflows/tests.yml/badge.svg)](https://github.com/tyler36/ddev-vite/actions/workflows/tests.yml)
[![last commit](https://img.shields.io/github/last-commit/tyler36/ddev-vite)](https://github.com/tyler36/ddev-vite/commits)

# ddev-vite <!-- omit in toc -->

- [What is ddev-vite?](#what-is-ddev-vite)
- [Components of the repository](#components-of-the-repository)
- [Getting started](#getting-started)
  - [Automatically start ViteJs](#automatically-start-vitejs)

## What is ddev-vite?

[ViteJs](https://vitejs.dev/) is a modern bundler, designed to be fast.

This add-on is a low-config option that exposes Vite's default port from DDEV so you can view the page in your browser.
It is based on the article [Working with Vite in DDEV - an introduction](https://ddev.com/blog/working-with-vite-in-ddev/), by Matthias Andrasch.
Developers are responsible for installing, maintaining, and running the Vite server. This add-on only exposes the port.

## Components of the repository

- [config.vite.yaml](config.vite.yaml): configure DDEV to expose the port.
- [install.yaml](install.yaml): describes how to install the service or other component.
- [test.bats](tests/test.bats): test suite to confirm add-on continues to work as expected.
- [Github actions setup](.github/workflows/tests.yml): automates daily tests and on pull requests.

## Getting started

This add-on assumes the developer has:

- Installed ViteJs via their preferred package manager.
- A valid ViteJS configuration file is present in the project root.

1. Install the add-on and restart DDEV

```shell
ddev add-on get tyler36/ddev-vite
ddev restart
```

1. Update `vite.config.js`

```js
const port = 5173;
const origin = `${process.env.DDEV_PRIMARY_URL}:${port}`;

export default defineConfig({
    ...
    // Adjust Vites dev server for DDEV: https://vitejs.dev/config/server-options.html
    server: {
        // The following line is require until the release of https://github.com/vitejs/vite/pull/19241
        cors: { origin: process.env.DDEV_PRIMARY_URL },
        // ----------------
        host: '0.0.0.0',
        port: port,
        origin: origin,
        strictPort: true
    },
});
```

1. Start Vite inside the container.

```shell
ddev npn run dev
```

### Automatically start ViteJs

To automatically start the ViteJs server, update DDEV `post-start` hook in `.ddev/config.yaml`.
For example:

```yaml
hooks:
  post-start:
    - exec: npm run dev
```

**Contributed and maintained by [@tyler36](https://github.com/tyler36)**
