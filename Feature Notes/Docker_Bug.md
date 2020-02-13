Production Bug : ModuleNotFoundError: Module not found: Error: Can't resolve '../../../node_modules/@patternfly/patternfly/images/chris_ui_background_filter.svg' in '/app/src/assets/scss'

Some observations:

Webpacked assets

### Handling Static Assets

All our templates and CSS are parsed by a css-loader or a vue-html-loader (for Vue Components) to look for asset URLs.
For example:

```html
<img src="./logo.png" />
```
and

```css
background:url(./logo.png)
```


Because logo.png is not JS, when treated as a module dependency, we need to use url-loader and file-loader to process it.  Since these assets may be inlined/copied/renamed during build, they are essentially part of your source code.
This is why it is recommended to place Webpack-processed static assets inside src/, alongside other source files. 

### Asset Resolving Rules

1. Relative URLs: e.g ./assets/logo/png will be interpretes as a module dependency. They will be replaced with an auto-generated URL based on your Webpack output configuration.

2. Non-Prefixed URLs: e.g. assets/logo.png will be treated the same as the relative URLs and translated into ./assets/logo.png.

3. URLs prefixed with ~ are treated as a module request, similar to requre('some-module/image.png'). You need to use this prefix if you want to leverage Webpack's module resolving configurations. 

4. Root-relative URLs- e.g /assets/logo.png are not processed at all.



