<!-- Copyright 2015. Author: Jeffrey Hing. All Rights Reserved. MIT License -->

# AngularWidthQuery

AngularWidthQuery is a set of directives which applies HTML classes to 
one or more elements based upon a container element's width.

It was created to support fluid layouts in situations where the container
element's width changes due to the user opening and closing other panels in 
the UI.

AngularWidthQuery is a substitute for CSS media queries which applies
CSS rules based upon the screen width, rather than an element's width.


## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Example](#example)
- [Usage](#usage)
    - [Factory Method](#factory-method)
    - [WidthQuery Directive](#widthQuery-directive)
    - [WidtyQueryClass Directive](#widthQueryClass-directive)
   
## Features

* Tiny implementation.
* CSS transition and animation aware.
* Customizable directive names.
* Compatible with CommonJS, AMD, and non-module build environments.

## Installation

To install the package:

    npm install angular-width-query
    
To require the package:    

```javascript
var widthQueryDirective = require("angular-width-query");
```     

## Example

The easist way to understand AngularWidthQuery is to walk 
through a quick example of creating a fluid layout consisting of 6 
equally sized sections. At small width, the sections are stacked. 
At medium width, the sections are arranged in two columns. At large width,
the sections are arranged in three columns.

### 1. Add the Directives

First, add the directives from AngularWidthQuery to an Angular module.

```javascript
widthQueryDirective('app');
```    

### 2. Create the HTML Structure

Next, create the HTML structure for the container and the 6 sections.
The values passed to the directives are the base HTML classes that will
be automatically added to the elements.

```html
<div width-query="container">
    <div width-query-class="section">Section 1</div>
    <div width-query-class="section">Section 2</div>
    <div width-query-class="section">Section 3</div>
    <div width-query-class="section">Section 4</div>
    <div width-query-class="section">Section 5</div>
    <div width-query-class="section">Section 6</div>
</div>

```
### 3. Create the CSS

Lastly, create the CSS rules.

```css
.container {
    font-size: 0;
}

.section {
    border: 1px solid #c0c0c0;
    display: inline-block;
    font-size: 1rem;
    height: 300px;
}

.section-large {
    width: 33.33%;
}

.section-medium {
    width: 50%;
}

.section-small {
    width: 100%;
}
```

That's it. You're done. 

Of course, the classes, and how they are applied, are fully customizable.
See the Usage section below for the details.

## Usage

### Factory Method

The `widthQueryDirective()` factory method adds the AngularWidthQuery
directives to an Angular module.

To add the directives, call `widthQueryDirective()` with
the name of the Angular module, and the options:

```javascript
widthQueryDirective('app', {
    directiveNames: {
        widthQuery: <string>,
        widthQueryClass: <string>
    },
    modifiers: [
        [<string>, <number>, <number>],
        ...
    ],
    pollingInterval: <milliseconds>
});
```
#### Directive Names Option

This option allows the names of the widthQuery and widthQueryClass directives
to be changed to suit your needs.

```javascript
widthQueryDirective('app', {
    directiveNames: {
        widthQuery: 'myWidthQuery',
        widthQueryClass: 'myWidthQueryClass'
    }
});
```

#### Modifiers Option

This option allows you to specify which HTML class modifiers are added to the
base HTML class.

Each item in the array is an array consisting of the HTML class modifier, 
the lower width range, and the upper width range.

The widthQuery directive searches these ranges in-order to determine which class
modifer to apply to the element's base HTML class.

```javascript
widthQueryDirective('app', {
    modifiers: [
        ['small', 0, 767],
        ['medium', 768, 1023],
        ['large', 1024, 100000]
    ]
});
```

#### Polling Interval Option

This option allows you to specify the interval at which the directive polls
the element's width. The directive begins polling the element's width when
a window resize or Angular digest event occurrs. When the width stops 
changing, the polling stops. 

```javascript
widthQueryDirective('app', {
    pollingInterval: 100
});
```
### WidthQuery Directive

The widthQuery directive modifies the element's base HTML class based upon the 
element's width.

```html
<div width-query="container">
    ...
</div>
```

The widthQuery directive can be passed the base HTML class name, or a set of
options:

```html
<div width-query="{
    class: 'container',
    modifiers: [
        ['small', 0, 399],
        ['medium', 400, 1279],
        ['large', 1280, 100000]
    ],
    pollingInterval: 100
    widthPollingListener: 'ctrl.widthChanged(width)'
    widthListener: 'ctrl.widthChanged(width)'
}">...</div>
```

#### Class Option

This option specifies the base HTML class name to add to the element.

#### Modifiers Option

This option allows you to specify which HTML class modifiers are added to the
base HTML class.

Each item in the array is an array consisting of the HTML class modifier, 
the lower width range, and the upper width range.

The widthQuery directive searches these ranges in-order to determine which class
modifer to apply to the element's base HTML class.

```javascript
widthQueryDirective('app', {
    modifiers: [
        ['small', 0, 767],
        ['medium', 768, 1023],
        ['large', 1024, 100000]
    ]
});
```

#### Polling Interval Option

This option allows you to specify the interval at which the directive polls
the element's width. The directive begins polling the element's width when
a window resize or Angular digest event occurrs. When the width stops 
changing, the polling stops. 

```javascript
widthQueryDirective('app', {
    pollingInterval: 100
});
```
#### Width Listener Option

This option allows you to specify a function to call when the width has
stopped changing. This is useful when you need to update the display of other
elements when an animation or transition has completed.

```javascript
widthQueryDirective('app', {
    widthListener: 'ctrl.widthChanged(width)'
});
```

#### Width Polling Listener Option

This option allows you to specify a function to call while the width is 
changing. This is useful when you need to update the display of other
elements during an animation or transition.

```javascript
widthQueryDirective('app', {
    widthPollingListener: 'ctrl.widthChanging(width)'
});
```
