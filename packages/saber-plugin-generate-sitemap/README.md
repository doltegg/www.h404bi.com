# saber-plugin-generate-sitemap

Sitemap generator plugin for [Saber.js](https://saberjs.org/).

## Install

```bash
yarn add saber-plugin-generate-sitemap
```

## Usage

In your `saber-config.yml`:

```yml
plugins:
  - resolve: saber-plugin-generate-sitemap
    options:
      # The output path of the sitemap (optional)
      path: '/sitemap.xml'
      # Path of your custom sitemap template (optional)
      template: './sitemap.xml'
```

### Saber Variables

This plugin will also inject some useful Saber variables you can use at runtime:

```js
import variables from 'saber/variables'

variables.sitemap
// true

variables.sitemapLink
// The link to the generated sitemap
```

## Options

### path

- Type: `string`
- Default: `sitemap.xml`

The output path of the sitemap. output to `<websiteRoot>/sitemap.xml` by default.

### template

- Type: `string`
- Default: `sitemap.xml`

Path of your custom sitemap template, relative path to your project root.
The template should be driven by [ejs](https://www.ejs.co/) template engine.

## License

[MIT](LICENSE) © h404bi

> [Website](https://www.h404bi.com) · GitHub [@h404bi](https://github.com/h404bi) · Twitter [@h404bi](https://twitter.com/h404bi)
