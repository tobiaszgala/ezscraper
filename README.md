# EZscraper
Simple, low-dependency web scraping module for nodejs.

## About
EZscraper is a simple web scraping module build on top of well maintained JSDOM module. It was created while working on showcase project for Treehouse tech degree. Feel free to contribute and also... please share your projects with me!

## Installation
```
npm install --save ezscraper
```
## Documentation

## Technical details

EZscraper is build on top of JSDOM module. It's vanilla JS friendly so any property should be speficied like regular property from  It uses querySelectorAll to select specified elements on the page. It also support 

```
ezscraper( url, { options } )
```

## Params
- **String** *url* : The url of the page we want to collect data.
- **Object** *options* : Options to help scraper collect data.
  - *Selector* - Valid CSS selector
  - *Property* - Property we want to retrieve (textContent by default)

## Returns
- A **promise object** containing scraped data.

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

By default, if only valid CSS selector is provided without any additional options, textContent is default property a script is trying to retrieve.

```javascript
const ezscraper = require('ezscraper');

ezscraper('http://super-amazing-website.com/', {
  title : 'div.container h1.title'
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
  link : {
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
  node : {
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

Each option returns array of objects. Useful when selecting multiple elements.

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


// will update documentation soon!
