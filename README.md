Showdown's Soundcloud Extension
==========================

------

**An extension to embed [Soundcloud](https://soundcloud.com/uiceheidd/robbery) in showdown documents using markdown syntax**

## Introduction

This extension enables embedding soundcloud sounds using markdown's image syntax. Instead of creating a new definition,
it borrows image syntax `![foo][bar.jpg]` to create an inframe pointing to the desired video.
Although it uses the same syntax as images, only valid soundcloud sound links are actually parsed.
Basically it looks for urls that start with:
 - `https://soundcloud.com/uiceheidd/robbery` or
 - `http://snd.sc/1048eaY`

## Installation

### With [npm](http://npmjs.org)

    npm install showdown-soundcloud-wrapper

## Enabling the extension

After including the extension in your application, you just need to enable it in showdown.

    var converter = new showdown.Converter({extensions: ['soundcloud']});

When using in node, ensure to first require the extension so it can register itself with showdown before any converters try to use it.

```javascript
var showdown = require('showdown');
require('showdown-soundcloud-wrapper');
var converter = new showdown.Converter({extensions: ['soundcloud']});
```

## Example

```javascript
var converter = new showdown.Converter({extensions: ['soundcloud']}),
    input = '![https://soundcloud.com/uiceheidd/robbery](https://soundcloud.com/uiceheidd/robbery)';
    html = converter.makeHtml(input);
console.log(html);
```

This should output the equivalent to:

```html
<a href="https://soundcloud.com/uiceheidd/robbery" target="_blank"></iframe>
```

## Wrapper
To use a wrapper for your iframe you can specify the following options after you define your converter:
```javascript
var converter = new showdown.Converter({extensions: ['soundcloud']});

converter.setOption('sc-useWrapper', true); // uses the default div wrapper

input = '![https://soundcloud.com/uiceheidd/robbery](https://soundcloud.com/uiceheidd/robbery)';
var html = converter.makeHtml(input);
console.log(html);
```
The default wrapper is a div with showdown-soundcloud-embed-wrapper no id and class. To change these the following can be specified after setting the converter:
```javascript
var converter = new showdown.Converter({extensions: ['soundcloud']});

converter.setOption('sc-useWrapper', true); // uses the default wrapper
converter.setOption('sc-wrapperEl', 'p'); // changes the default div wrapper to a paragraph
converter.setOption('sc-enableWrapperId', true); // enables the wrapper to have a custom Id
converter.setOption('sc-wrapperId', 'soundcloud-embed'); // changes the default showdown-soundcloud-embed-wrapper id to soundcloud-embed
converter.setOption('sc-enableWrapperClass', true); // enables the wrapper to have a custom class
converter.setOption('sc-wrapperClass', 'soundcloud-embeds'); // changes the default showdown-soundcloud-embed-wrapper id to soundcloud-embeds

input = '![https://soundcloud.com/uiceheidd/robbery](https://soundcloud.com/uiceheidd/robbery)';
var html = converter.makeHtml(input);
console.log(html);
```
as Soundcloud requires an id for the iframe, we cannot manually place iframe with showdown. We'll need to make a request to Soundcloud API from frontend. Replace LINK_HERE with your link:
`http://soundcloud.com/oembed?format=json&url=LINK_HERE&iframe=true`
then you can send the html inside the response to the following snippet:

The following Jquery snippet can be accompanied with your wrapper to replace the link with the iframe on the frontend
```javascript
    $(".soundcloud-embeds").map(async (index, element) => {
      const link = $(element).find(">:first-child").attr("href");
      const html; // make a request to the above url and set the html in the response to a variable then replace it with the below code.
      $(element).find(">:first-child").replaceWith($(result.data.html));
    });
```
If you change the default ID, make sure to change the CSS aswell.


## Options

Soundcloud extension reads the options from Showdown Core. So, to pass an option, use the methods explained in
[Showdown's documentation](https://github.com/showdownjs/showdown#setting-options).


## License
These files are distributed under BSD license. For more information,
please check the [LICENSE file](https://github.com/honest-cash/showdown-soundcloud-extension/blob/master/LICENSE) in the source code.
