# Help.com JavaScript Style Guide() {

Based off of [Airbnb's style guide](https://github.com/airbnb/javascript).

Our linter files
- [jshintrc](linters/jshintrc)
- [jsbeautifyrc](linters/jsbeautifyrc)
- [SublimeLinter.sublime-settings](linters/SublimeLinter.sublime-settings)

## Table of Contents

  1. [Types](#types)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Requires](#requires)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Events](#events)
  1. [Ternaries](#ternaries)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [Testing](#testing)
  1. [Resources](#resources)
  1. [License](#license)

## Types

  - [1.1](#1.1) <a name='1.1'></a> **Primitives**: When you access a primitive type you work directly on its value.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - [1.2](#1.2) <a name='1.2'></a> **Complex**: When you access a complex type you work on a reference to its value.

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## Objects

  - [2.1](#2.1) <a name='2.1'></a> Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - [2.2](#2.2) <a name='2.2'></a> If your code will be executed in browsers in script context, don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61). It’s OK to use them in ES6 modules and server-side code.

    ```javascript
    // bad
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // good
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - [2.3](#2.3) <a name='2.3'></a> Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    var superman = {
      class: 'alien'
    };

    // bad
    var superman = {
      klass: 'alien'
    };

    // good
    var superman = {
      type: 'alien'
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - [3.1](#3.1) <a name='3.1'></a> Use the literal syntax for array creation.

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - [3.2](#3.2) <a name='3.2'></a> Use Array#push instead of direct assignment to add items to an array.

    ```javascript
    var someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - [3.3](#3.3) <a name='3.3'></a> When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - [3.4](#3.4) <a name='3.4'></a> To convert an array-like object to an array, use Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  - [4.1](#4.1) <a name='4.1'></a> Use single quotes `''` for strings.

    ```javascript
    // bad
    var name = "Capt. Janeway";

    // good
    var name = 'Capt. Janeway';
    ```

  - [4.2](#4.2) <a name='4.2'></a> Strings longer than 80 characters should be written across multiple lines using string concatenation.
  - [4.3](#4.3) <a name='4.3'></a> Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - [4.4](#4.4) <a name='4.4'></a> When programmatically building up a string, use Array#join instead of string concatenation. Mostly for IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // bad
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // good
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Functions

  - [5.1](#5.1) <a name='5.1'></a> Function expressions:

    ```javascript
    // bad
    var anonymous = function() {
      return true;
    };

    // good
    var named = function named() {
      return true;
    };
    ```

  - [5.2](#5.2) <a name='5.2'></a> Function expressions:

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - [5.3](#5.3) <a name='5.3'></a> Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
  - [5.4](#5.4) <a name='5.4'></a> **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - [5.5](#5.5) <a name='5.5'></a> Avoid abbreviating parameter names

    ```javascript
    // bad
    function nope(err, req, res, nxt) {
      // ...stuff...
    }

    // good
    function yup(error, request, response, next) {
      // ...stuff...
    }
    ```

  - [5.6](#5.6) <a name='5.6'></a> Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, parameters) {
      // ...stuff...
    }
    ```

  - [5.7](#5.7) <a name='5.7'></a> Always return a function's value as early as possible.

    ```js
    // bad
    function isPercentage(val) {
      if (val >= 0) {
        if (val < 100) {
          return true;
        } else {
          return false;
        }
      } else {
        return false;
      }
    }

    // good
    function isPercentage(val) {
      if (val < 0) {
        return false;
      }

      if (val > 100) {
        return false;
      }

      return true;
    }

    // even better
    function isPercentage(val) {
      var isInRange = (val >= 0 && val <= 100);
      return isInRange;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Constructors

  - [6.1](#6.1) <a name='6.1'></a> Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - [6.2](#6.2) <a name='6.2'></a> Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  - [6.3](#6.3) <a name='6.3'></a> It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Modules

  - [7.1](#7.1) <a name='7.1'></a> The module should start with a `!`. This ensures that if a malformed module forgets to include a final semicolon there aren't errors in production when the scripts get concatenated. [Explanation](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - [7.2](#7.2) <a name='7.2'></a> The file should be named with camelCase, live in a folder with the same name, and match the name of the single export.
  - [7.3](#7.3) <a name='7.3'></a> Add a method called `noConflict()` that sets the exported module to the previous version and returns this one.
  - [7.4](#7.4) <a name='7.4'></a> Always declare `'use strict';` at the top of the module.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

**[⬆ back to top](#table-of-contents)**

## Properties

  - [8.1](#8.1) <a name='8.1'></a> Use dot notation when accessing properties.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - [8.2](#8.2) <a name='8.2'></a> Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ back to top](#table-of-contents)**

## Variables

  - [9.1](#9.1) <a name='9.1'></a> Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - [9.2](#9.2) <a name='9.2'></a> Use one `var` declaration for every variable and declare each variable on a newline.

    > Why? It's easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs.

    ```javascript
    // bad
    var items = getItems(),
      goSportsTeam = true,
      dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - [9.3](#9.3) <a name='9.3'></a> Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i;
    var len;
    var dragonball;
    var items = getItems();
    var goSportsTeam = true;

    // bad
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - [9.4](#9.4) <a name='9.4'></a> Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

    ```javascript
    // bad
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // good
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Requires

  - [10.1](#10.1) <a name='10.1'></a> Organize your node requires in the following order:
      - core modules
      - npm modules
      - others

    ```javascript
    // bad
    var Car = require('./models/Car');
    var async = require('async');
    var http = require('http');

    // good
    var http = require('http');
    var fs = require('fs');
    var async = require('async');
    var mongoose = require('mongoose');
    var Car = require('./models/Car');
    ```

  - [10.2](#10.2) <a name='10.2'></a> Do not use the `.js` when requiring modules

  ```javascript
    // bad
    var Batmobile = require('./models/Car.js');

    // good
    var Batmobile = require('./models/Car');
  ```

**[⬆ back to top](#table-of-contents)**

## Hoisting

  - [11.1](#11.1) <a name='11.1'></a> `var` declarations get hoisted to the top of their scope, their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - [11.2](#11.2) <a name='11.2'></a> Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - [11.3](#11.3) <a name='11.3'></a> Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - [11.4](#11.4) <a name='11.4'></a> Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ back to top](#table-of-contents)**

## Comparison Operators & Equality

  - [12.1](#12.1) <a name='12.1'></a> Use `===` and `!==` over `==` and `!=`.
  - [12.2](#12.2) <a name='12.2'></a> Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - [12.3](#12.3) <a name='12.3'></a> Do not use shortcuts.

    ```javascript
    // bad
    if (name) {
      // ...stuff...
    }

    // good
    if (name !== '') {
      // ...stuff...
    }

    // bad
    if (collection.length) {
      // ...stuff...
    }

    // good
    if (collection.length > 0) {
      // ...stuff...
    }
    ```

  - [12.4](#12.4) <a name='12.4'></a> For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ back to top](#table-of-contents)**

## Blocks

  - [13.1](#13.1) <a name='13.1'></a> Use braces on all blocks.
  - [13.2](#13.2) <a name='13.2'></a> Do not use single-line blocks.

    ```javascript
    // bad
    if (test)
      return false;

    // bad
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  - [14.1](#14.1) <a name='14.1'></a> Use `/** ... */` for multi-line comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - [14.2](#14.2) <a name='14.2'></a> Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // not preferred, but tolerable in limited uses
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - [14.3](#14.3) <a name='14.3'></a> Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.

  - [14.4](#14.4) <a name='14.4'></a> Use `// FIXME:` to annotate problems.

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - [14.5](#14.5) <a name='14.5'></a> Use `// TODO:` to annotate solutions to problems.

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Whitespace

  - [15.1](#15.1) <a name='15.1'></a> Use soft tabs set to 2 spaces.

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙var name;
    }
    ```

  - [15.2](#15.2) <a name='15.2'></a> Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - [15.3](#15.3) <a name='15.3'></a> Set off operators with spaces.

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - [15.4](#15.4) <a name='15.4'></a> End files with a single newline character.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - [15.5](#15.5) <a name='15.5'></a> Use indentation when making long method chains.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  - [16.1](#16.1) <a name='16.1'></a> Leading commas: **Nope.**

    ```javascript
    // bad
    var story = [
        once
      , upon
      , aTime
    ];

    // good
    var story = [
      once,
      upon,
      aTime
    ];

    // bad
    var hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    var hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers'
    };
    ```

  - [16.2](#16.2) <a name='16.2'></a> Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    var hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // good
    var hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ back to top](#table-of-contents)**

## Semicolons

  - [17.1](#17.1) <a name='17.1'></a> **Yup.**

    ```javascript
    // bad
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ back to top](#table-of-contents)**

## Type Casting & Coercion

  - [18.1](#18.1) <a name='18.1'></a> Perform type coercion at the beginning of the statement.
  - [18.2](#18.2) <a name='18.2'></a> Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - [18.3](#18.3) <a name='18.3'></a> Use `parseInt` for Numbers and always with a radix for type casting.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - [18.4](#18.4) <a name='18.4'></a> If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - [18.5](#18.5) <a name='18.5'></a> **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - [18.6](#18.6) <a name='18.6'></a> Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  - [19.1](#19.1) <a name='19.1'></a> Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - [19.2](#19.2) <a name='19.2'></a> Use camelCase when naming objects, functions, and instances.

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - [19.3](#19.3) <a name='19.3'></a> Use PascalCase when naming constructors or classes.

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - [19.4](#19.4) <a name='19.4'></a> Use ALL_UPPERCASE when naming constants
    ```javascript
    // bad
    function File() {
      // ..stuff..
    }
    File.fullPermissions = 0777;

    // good
    function File() {
      // ..stuff..
    }
    File.FULL_PERMISSIONS = 0777;
    ```

  - [19.5](#19.5) <a name='19.5'></a> Use a leading underscore `_` when naming private properties.

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - [19.6](#19.6) <a name='19.6'></a> When saving a reference to `this` use `self`.

    ```javascript
    // bad
    function foo() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }

    // bad
    function foo() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function foo() {
      var self = this;
      return function() {
        console.log(self);
      };
    }
    ```

  - [19.7](#19.7) <a name='19.7'></a> Name your functions. This is helpful for stack traces.

    ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - **Note:** IE8 and below exhibit some quirks with named function expressions.  See [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/) for more info.

**[⬆ back to top](#table-of-contents)**

## Accessors

  - [20.1](#20.1) <a name='20.1'></a> Accessor functions for properties are not required.
  - [20.2](#20.2) <a name='20.2'></a> If you do make accessor functions use getVal() and setVal('hello').

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - [20.3](#20.3) <a name='20.3'></a> If the property is a `boolean`, use `isVal()` or `hasVal()`.

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - [20.4](#20.4) <a name='20.4'></a> It's okay to create get() and set() functions, but be consistent.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Events

  - [21.1](#21.1) <a name='21.1'></a> When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId: listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ back to top](#table-of-contents)**

## Ternaries

  - [22.1](#22.1) <a name='22.1'></a> Always multi-line ternary operators

    ```javascript
    // bad
    var foo = (a === b) ? 1 : 2;

    // good
    var foo = (a === b)
      ? 1
      : 2;
    ```

**[⬆ back to top](#table-of-contents)**

## jQuery

  - [23.1](#23.1) <a name='23.1'></a> Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');

    // good
    var $sidebarBtn = $('.sidebar-btn');
    ```

  - [23.2](#23.2) <a name='23.2'></a> Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - [23.3](#23.3) <a name='23.3'></a> For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - [23.4](#23.4) <a name='23.4'></a> Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**

## ECMAScript 5 Compatibility

  - [24.1](#24.1) <a name='24.1'></a> Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/).

**[⬆ back to top](#table-of-contents)**

## Testing

  - [25.1](#25.1) <a name='25.1'></a> **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Resources

See [Airbnb Recommended Resources](https://github.com/airbnb/javascript#resources) for resources.

**[⬆ back to top](#table-of-contents)**

## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team's style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.

# };
