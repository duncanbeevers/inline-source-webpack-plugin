<a href="https://www.npmjs.com/package/inline-source-webpack-plugin"><img src="https://img.shields.io/npm/v/inline-source-webpack-plugin.svg" alt="npm Version"></a>
[![Build Status](https://travis-ci.org/KyLeoHC/inline-source-webpack-plugin.svg?branch=master)](https://travis-ci.org/KyLeoHC/inline-source-webpack-plugin)

# inline-source-webpack-plugin

A webpack plugin to embed css/js resource in the html with inline-source module([html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin) is needed).

## Install

```bash
$ npm i inline-source-webpack-plugin -D
```

## example

```html
<!-- ./demo/src/index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>test</title>
    <link href="inline.css" inline="true">
    <script src="inline.js" inline="true"></script>
  </head>
  <body>
    <div class="container">
      <h1>hello world!</h1>
    </div>
    <!-- 'inline-asset' attribute tell us to embed file that generated by webpack -->
    <script inline inline-asset="runtime\.\w+\.js$" inline-asset-delete></script>
    <script inline inline-asset="bundle\.\w+\.js$" inline-asset-delete></script>
  </body>
</html>
```

You can find this demo in the demo directory and view the output:

```bash
# install dependency
npm i

# enter the demo directory
cd demo

# build
npm run build
```

## Usage

Available options include(refer to [this](https://github.com/popeindustries/inline-source#usage) for more options):

- `compress`: enable/disable compression.(default `false`)
- `rootpath`: path used for resolving inlineable paths.
- `noAssetMatchReplace`: work with `noAssetMatch` option.(default `<!-- -->`)
- `noAssetMatch`: define the behaviour while no asset match the value of `inline-asset` attribute.(default `1`)
  - `0`: do nothing and the tag is still reserved in the html.
  - `1`: throw warning tips and replace the tag with the content of `noAssetMatchReplace` option.
  - `2`: throw error tips and replace the tag with the content of `noAssetMatchReplace` option(This level will affect the compilation of webpack).

```javascript
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const InlineSourceWebpackPlugin = require('inline-source-webpack-plugin');

module.exports = {
  ...,
  plugins: [
    new HtmlWebpackPlugin({
      ...
    }),
    new InlineSourceWebpackPlugin({
      compress: true,
      rootpath: './src'
    })
  ]
};
```

If you want to embed the files that generated by webpack or other plugin, you can use `inline-asset` attribute to filter the files(Please don't try to use `src` or `href`).
Add `inline-asset-delete` attribute for deleting the asset after inline task.

```html
<script inline inline-asset-delete inline-asset="Your asset path/Your asset name"></script>
```

The value of `inline-asset` attribute is a *regular expression*.
  
Note: For `inline-asset` feature, you may notice the 'no asset match' warning or error in developement mode as you write the regular expression for the production mode.Just ignore the 'no asset match' warning or error while in developement mode.Or you can provide `noAssetMatch` option for ignoring the warning or error;

## License

[MIT License](https://github.com/KyLeoHC/inline-source-webpack-plugin/blob/master/LICENSE)
