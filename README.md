<p style="text-align: center">
  <img src="./.github/banner.svg">
</p>

<!-- [![d](https://img.shields.io/npm/dm/nuxt-vite.svg?style=flat-square)](https://npmjs.com/package/nuxt-vite) -->
[![v](https://img.shields.io/npm/v/nuxt-vite/latest.svg?style=flat-square)](https://npmjs.com/package/nuxt-vite)
[![a](https://img.shields.io/github/workflow/status/nuxt/vite/ci/main?style=flat-square)](https://github.com/nuxt/vite/actions)
[![c](https://img.shields.io/codecov/c/gh/nuxt/vite/main?style=flat-square)](https://codecov.io/gh/nuxt/vite)


**🧪 Vite mode is experimental and many nuxt modules are still incompatible**

**If found a bug, please report via [issues](https://github.com/nuxt/vite/issues) with a minimal reproduction**

<!-- [![See Demo](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/github/nuxt/vite/tree/main/demo) -->

## What is working?

- [x] Using vite in development
- [x] Basic server-side rendering
- [x] Basic Hot-Module-Replacement
- [x] Nuxt plugins
- [x] Nuxt components
- [X] Vuex store
- [x] Page middleware
- [ ] Composition API
- [ ] Content module

## Usage

Install package:

```sh
yarn add --dev nuxt-vite
# OR
npm i -D nuxt-vite
```

Add to `buildModules`:

```js
// nuxt.config
export default {
  buildModules: [
    'nuxt-vite'
  ]
}
```

**Note:** Nuxt >= 2.15.0 is required

## How it works

Nuxt uses has a powerful hooking system to extend internals and abstracted bundler ([@nuxt/webpack](https://github.com/nuxt/nuxt.js/tree/dev/packages/webpack)) which can be replaced.
Vite module, replaces webpack by a similar interface to use vite instead of webpack. Client-side modules are loaded on demand using vite middleware.

Server-side bundle is being created by another vite instance and written to filesystem and passed using hooks to nuxt server-renderer.
Current approach is not most efficient due to usage of filesystem, extra build and lack of lazy loading.
Yet much faster than webpack builds. You can opt-out SSR build using `nuxt dev --spa`

This module could not be possible without [vite-plugin-vue2](https://github.com/underfin/vite-plugin-vue2) by [@underfin](https://github.com/underfin)

### Graphql - workaround for importing .gql files

There is a known bug when loading .gql files ([#31](https://github.com/nuxt/vite/issues/31)). Best solution for now is to wrap gql files with .js extension along with importing [graphql-tag](https://www.npmjs.com/package/graphql-tag) or using raw GraphQL queries. Remember to add `loc.source.body`.

*Example path: /apollo/queries/products.js*
```
import gql from 'graphql-tag'

export default gql`
  query Products {
    products {
      id
      name
    }
  }
`
```

*Example path: /pages/index.vue*
```
import products  from '~/queries/products'

export default {
    async asyncData({ $strapi }) {
        const response = await $strapi.graphql({
            query: products.loc.source.body,
        })
        return {
            response
        }
    }
}
```
## License

MIT - Nuxt Team
