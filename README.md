# markdown-it-include

[![Build Status](https://img.shields.io/travis/GerHobbelt/markdown-it-include/master.svg?style=flat)](https://travis-ci.org/GerHobbelt/markdown-it-include)
[![NPM version](https://img.shields.io/npm/v/@gerhobbelt/markdown-it-include.svg?style=flat)](https://www.npmjs.org/package/@gerhobbelt/markdown-it-include)
[![Coverage Status](https://img.shields.io/coveralls/GerHobbelt/markdown-it-include/master.svg?style=flat)](https://coveralls.io/r/GerHobbelt/markdown-it-include?branch=master)

Markdown-it plugin which adds the ability to include markdown fragment files.

## Install

node.js, browser:

```bash
npm install markdown-it-include --save
bower install markdown-it-include --save
```

## Use

Let's create a markdown which uses a header and a footer from two separate files:

### File: '**header.md**'

```markdown
# This is my header for all my markdowns
```

### File: '**footer.md**'

```markdown
Follow me on twitter!
```

Let's assume that header.md and footer.md are located in `/in/this/directory`.

Now it's your turn to play markdown-it!

```js
var md = require('markdown-it')()
            .use(require('markdown-it-include'), '/in/this/directory');

md.render('!!!include(header.md)!!!\n\n*your content*\n\n!!!include(footer.md)!!!');
```

It will produce this

```html
<h1>This is my header for all my markdowns</h1>
<p><em>your content</em></p>
<p>Follow me on twitter!</p>
```

## Options

```js
var md = require('markdown-it')()
            .use(require('markdown-it-include'), options);
```

* Options Type: `String|Object`

If it's a string, it's the same as `options.root`.

Otherwise, it must be an options object (or `null` / `undefined` when you only wish to use the default options).

The options object may contain any of these:

### root

* Type: `String`
* Default: `.`

`root` is the base directory of all the markdown files.

### includeRe

* Type: `RegExp`
* Default: `/\!{3}\s*include(.+?)\!{3}/i`

By default the `!!!include(path)!!!` statement is used to include markdown fragment files. This option allows to change the regular expression and then customize this statement.

### bracesAreOptional

* Type: `Boolean`
* Default: `false`

When set to TRUE, this option allows users to write include statements like `!!!include path !!!` instead of ``!!!include(path)!!!`. Note that when this option is set, both these forms are acceptable.

> This allows you to configure the plugin to use C/C++ like `#include` statements in your MarkDown if you're so inclined -- I know I am! :)

Example usage:

```js
const options = {
  includeRe: /#include(.+)/,
  bracesAreOptional: true
};

let md = require('markdown-it')()
           .use(require('markdown-it-include'), options);
let html = md.render('#include(xyz.md)\n');
```

### throwError

* Type: `Boolean`
* Default: `true`

When set to `false`, instead of throwing an error message, the error message will be written into the output. For references to possible error messages as well as how to change it, see options 'notFoundMessage' and 'circularMessage'.

Error messages will always be prefixed with the text `"INCLUDE ERROR:"` and will be rendered as `<h1>` headings wheenever possible so they will be pretty obvious in the generated output when they occur. After all, you don't want errors to silently hide under the rug.

### notFoundMessage

* Type: `String`
* Default: `File '{{FILE}}' not found.`

With `notFoundMessage` the default error message when the to be included file cannot be found can be changed. The marker `{{FILE}}` in the message string will be replaced with the full file path.

### circularMessage

* Type: `String`
* Default: `Circular reference between '{{FILE}}' and '{{PARENT}}'.`

With `circularMessage` the default error message when there is a circular reference between files can be changed. The markers `{{FILE}}` and `{{FILE}}` in the message string will be replaced with the respective full file paths.


## Disclaimer

This purposefully doesn't conform to any spec or discussion related to CommonMark.

## License

[MIT](https://github.com/camelaissani/markdown-it-include/LICENSE)
