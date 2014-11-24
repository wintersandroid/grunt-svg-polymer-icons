# grunt-svgstore

> Merge SVGs from a folder into a polymer core-svg-icons element.

## Why?
Because we needed it...

## Getting Started
This plugin requires Grunt `~0.4.5`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install wintersandroid/grunt-svg-polymer-icons --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-svg-polymer-icons');
```



## The "svgpolymericon" task

### Overview
In your project's Gruntfile, add a section named `svgpolymericon` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  svgpolymericon: {
    options: {
      prefix : 'icon-', // This will prefix each ID
      svg: {
        iconWidth : 40, // optional set the iconWidth property on the element
        iconHeight: 35, // option set the iconWidth property on the element
        iconSize: 30, // optional, set the iconSize property on the element.

      },
      polymer: {
        iconsetImport: "/shared_components/core-iconset-svg.html", // the iconset import
        iconsetElement: "core-iconset-svg" // the element to extend.

      },
    },
    your_target: {
      // Target-specific file lists and/or options go here.
    },
  },
});
```

### Options

#### options.prefix
Type: `String`  
Default value: `''`  

A string value that is used to prefix each filename to generate the id.

#### options.svg
Type: `Object`  
Default value: `{}`  

An object that is used to generate attributes for the resulting `svg` file.
```js
{
  iconWidth : 40, // optional set the iconWidth property on the element
  iconHeight: 35, // option set the iconWidth property on the element
  iconSize: 30, // optional, set the iconSize property on the element.
}
```
will result in:

```svg
<svg viewBox="0 0 100 100">
[...]
```

#### options.symbol 
Type: `Object`  
Default value: `{}`  

Just like `options.svg` but will add attributes to each generated `<symbol>`.

#### options.formatting
Type: `Object` or `boolean`  
Default value: `false`  

Formatting options for generated code.

To format the generated HTML, set `formatting` with [options](https://github.com/einars/js-beautify#options) like: `{indent_size : 2}`, which in context looks like:

```js
default: {
  options: {
    formatting : {
      indent_size : 2
    }
  }
```
See [js-beautify](https://github.com/einars/js-beautify) for more options.


#### options.cleanup
Type: `boolean`  or `Array`
Default value: `false`  

This option can be used to clean up inline definitions that may jeopardise later CSS-based styles.  
When set to true clean up all inline `style` attributes.  
Apart from true / false, the value of this property can be an array of inline attributes ( like `fill`, `stroke`, ...) you want to remove from the elements in the SVG.

#### options.cleanupdefs
Type: `boolean`  
Default value: `false`  

When set to false, no cleanup is performed on the `<defs>` element.

#### options.convertNameToId
Type: `function`

The function used to generate the ID from the file name. The function receives the file name without the `.svg` extension as its only argument.

The default implementation:
```js
function(name) {
  var dotPos = name.indexOf('.');
  if ( dotPos > -1){
    name = name.substring(0, dotPos);
  }
  return name;
}
```

#### options.fixedSizeVersion
Type: `Object` or `boolean`
Default value: `false`

When `true` or a configuration object is passed for each of the symbols another one, with suffixed id generated.
All those additional symbols have the common dimensions and refers to the original symbols with `<use>`.
Original symbols are placed exactly in the middle of the fixed-size viewBox of the fixed size version.

Configuration reference and default values if `true` is passed:
```js
grunt.initConfig({
  svgstore: {
    options: {
      fixedSizeVersion: {
        width: 50,
        height: 50,
        suffix: '-fixed-size',
        maxDigits: {
          translation: 4,
          scale: 4,
        },
      },
    },
  },
});
```

Any of the configuration object properties may be omitted.

### Usage Examples

This example will merge all elements from the `svgs` folder into the `<defs>`-Block of the `dest.svg`. You can use that SVG in HTML like:

```html
<!-- Include dest.svg -->

[...]

<svg><use xlink:href="#filename" /></svg>
````

```js
grunt.initConfig({
  svgstore: {
    options: {},
    default : {
      files: {
        'dest/dest.svg': ['svgs/*.svg'],
      },
    },
  },
});
```

## Supplemental Features

There are some hidden features available in grunt-svgstore:

  * Use the `preserve--` prefix (in the source SVG), for any attributes that should be forced to remain in the resulting SVG. For example, `preserve--stroke` would result in just `stroke` in the resulting SVG. This happens whether or not you ask for that attribute to be *cleaned* via `cleanup`.
  * Using the value of `currentColor` on any property with the key `fill`, will result in that property remaining in the resulting SVG (regardless of whether or not you ask for `fill` to be *cleaned* via `cleanup`). This can be used to achieve *accent color* for you SVG instances by defining the font color on a parent element via the CSS `color` property.

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History

#### 0.1.0

  * Initial version, modified from grunt-svgstore
