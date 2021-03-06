# Custom Themes

::: tip
Theme components are subject to the same [browser API access restrictions](./using-vue.md#browser-api-access-restrictions).
:::

VuePress uses Vue single file components for custom themes. To use a custom layout, create a `.vuepress/theme` directory in your docs root, and then create a `Layout.vue` file:

```
.
└─ .vuepress
   └─ theme
      └─ Layout.vue
```

From there it's the same as developing a normal Vue application. It is entirely up to you how to organize your theme.

## Site and Page Metadata

The `Layout` component will be invoked once for every `.md` file in `docs`, and the metadata for the entire site and that specific page will be exposed respectively as `this.$site` and `this.$page` properties which are injected into every component in the app.

This is the value of `$site` of this very website:

``` json
{
  "title": "VuePress",
  "description": "Vue-powered Static Site Generator",
  "base": "/",
  "pages": [
    {
      "path": "/",
      "title": "VuePress",
      "frontmatter": {}
    },
    ...
  ]
}
```

`title`, `description` and `base` are copied from respective fields in `.vuepress/config.js`. `pages` contains an array of metadata objects for each page, including its path, page title (explicitly specified in YAML frontmatter or inferred from the first header on the page), and any YAML frontmatter data in that file.

This is the `$page` object for this page you are looking at:

``` json
{
  "path": "/custom-themes.html",
  "title": "Custom Themes",
  "headers": [/* ... */],
  "frontmatter": {}
}
```

If the user provided `themeConfig` in `.vuepress/config.js`, it will also be available as `$site.themeConfig`. You can use this to allow users to customize behavior of your theme - for example, specifying categories and page order. You can then use these data together with `$site.pages` to dynamically construct navigation links.

Finally, don't forget that `this.$route` and `this.$router` are also available as part of Vue Router's API.

## Content Outlet

The compiled content of the current `.md` file being rendered will be available as a special `<Content/>` global component. You will need to render it somewhere in your layout in order to display the content of the page. The simplest theme can be just a single `Layout.vue` component with the following content:

``` html
<template>
  <div class="theme-container">
    <Content/>
  </div>
</template>
```

## Using Theme from a Dependency

Themes can be published on npm in raw Vue SFC format as `vuepress-theme-xxx`.

To use a theme from an npm dependency, provide a `theme` option in `.vuepress/config.js`:

``` js
module.exports = {
  theme: 'awesome'
}
```

VuePress will attempt to locate and use `node_modules/vuepress-theme-awesome/Layout.vue`.
