# EZscraper
Simple, low-dependency web scraping module for nodejs.

## About
EZscraper is a simple web scraping module build on top of well maintained JSDOM module. It was created while working on the showcase project for Treehouse tech degree. Feel free to contribute and also... please share your projects with me!

## Installation
```
npm install --save ezscraper
```

To use module simply require it without any additional options.

```
const ezscraper = require('ezscraper');
```

## Documentation

## Technical details

EZscraper is built on top of JSDOM module. Any property should be specified just like you would retrieve it using vanilla JS.  It uses querySelectorAll to select specified elements on the page (returns array). It also supports DOM traversing (see examples).

```
ezscraper( url, { options } )
```

## Params
- **String** *url* : The URL of the page we want to collect data.
- **Object** *options* : Selectors and properties you want to access.
  - *Selector* - Valid CSS selector
  - *Property* - Property we want to retrieve (textContent by default)

## Returns
- A **promise object** containing scraped data (if resolved) or error message (if rejected). Use then-catch for data/error handling.

## Examples

### Example markup

```html
<div class="container">
  <h1 class="title">Example Title</h1>
  <a href="index.html">Click me</a>
  <span class="message">You are awsome!</span>
  Here is some text node that is not wrapped inside html tags.
</div>
```

### Example 1

By default, if only valid CSS selector is provided without any additional options, *textContent* is default property EZscraper is trying to retrieve.

```javascript
const ezscraper = require('ezscraper');

ezscraper('http://super-amazing-website.com/', {
  title : 'div.container h1.title' // Example 1
}).then( data => console.log(data))
  .catch( error => console.error(error))
  
  
// results

{
  title : [{
    title : 'Example Title'
    }]
}
```

### Example 2

You can provide *selector* and *property* keys for specific properties you want to retrieve.

```javascript
const ezscraper = require('ezscraper');

ezscraper('http://super-amazing-website.com/', {
  title : 'div.container h1.title',
  link : { // Example 2
    selector : 'div.container a',
    property : 'href'
  }
}).then( data => console.log(data))
  .catch( error => console.error(error))
  
  
// results

{
  title : [{
    title : 'Example Title'
    }],
  link : [{
    link : 'index.html'
  }]
}
```

### Example 3

EZscraper supports DOM traversing so you can combine different properties together to get to the specific property.

```javascript
const ezscraper = require('ezscraper');

ezscraper('http://super-amazing-website.com/', {
  title : 'div.container h1.title',
  link : {
    selector : 'div.container a',
    property : 'href'
  },
  node : { // Example 3
    selector : 'div.container span.message',
    property : 'nextSibling.textContent'
  }
}).then( data => console.log(data))
  .catch( error => console.error(error))
  
  
// results

{
  title : [{
    title : 'Example Title'
    }],
  link : [{
    link : 'index.html'
  }],
  node : [{
    node : 'Here is some text node that is not wrapped inside html tags.'
  }]
}
```

### Example 4

Each option returns an array of objects. Useful when selecting multiple elements.

```html
<span class="message">Message 1</span>
<span class="message">Message 2</span>
<span class="message">Message 3</span>
<span class="message">Message 4</span>
<span class="message">Message 5</span>
```

```javascript
const ezscraper = require('ezscraper');

ezscraper('http://super-amazing-website.com/', {
  message : 'span.message'
}).then( data => console.log(data))
  .catch( error => console.error(error))
  
  
// results

{
  message : [ { message : 'Message 1' }, 
              { message : 'Message 2' },
              { message : 'Message 3' },
              { message : 'Message 4' },
              { message : 'Message 5' } ]
}
```

### Example project

[Simple content scraper](https://github.com/tobiaszgala/Simple-content-scraper) - Script handles page crawling using EZscraper. Example of using Promise.all to retrieve data from multiple sources.

