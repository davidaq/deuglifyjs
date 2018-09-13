# Deuglify

A utiltiy for reversing minified JS files, not simply formating whitespaces but also tries to make code much more readable.

## Usage

### CLI

```
$ npm install -g deuglify
$ deuglify source.js > source.demin.js
$ cat source.js | deuglify > source.demin.js
```

### Module

```
const { deuglify } = require('deuglify')
const sourceCode = 'console.log("hello")'
const { code, ast } = deuglify(sourceCode)
```
## Feature

 - Expand sequence expression into seperate statement
 - Split variable declaration to seperate declaration
 - Expand conditional expression and logical expresssion into if statement
 - Move irrelevant variable delcaration out of for statemet init expression
 - Transform `!0` to `true` and `!1` to false
 - Rename one letter variables to globally unique variable name (disabled when eval or with statement exists)
 
## Example

Original source:

```
!(function (global, factory) {
  if (module && typeof module === 'object' && module.exports) {
    module.exports = factory();
  } else {
    global.fibonacci = factory();
  }
})(this, function () {
  var table = [1, 1];
  function fibonacci (n) {
    if (!table[n]) {
      table[n] = fibonacci(n - 1) + fibonacci(n - 2);
    }
    return table[n];
  }
  return fibonacci;
});
```

Minified:

```
!function(a,b){module&&"object"==typeof module&&module.exports?module.exports=b():a.fibonacci=b()}(this,function(){function a(c){return b[c]||(b[c]=a(c-1)+a(c-2)),b[c]}var b=[1,1];return a});
```

Formatted (with chrome):

```
!function(a, b) {
    module && "object" == typeof module && module.exports ? module.exports = b() : a.fibonacci = b()
}(this, function() {
    function a(c) {
        return b[c] || (b[c] = a(c - 1) + a(c - 2)),
        b[c]
    }
    var b = [1, 1];
    return a
});

```

Deuglified:

```
!function (a__q, b__w) {
  if (module && "object" == typeof module && module.exports) {
    module.exports = b__w();
  } else {
    a__q.fibonacci = b__w();
  }
}(this, function () {
  function a__e(c__t) {
    if (!b__r[c__t]) {
      b__r[c__t] = a__e(c__t - 1) + a__e(c__t - 2);
    }

    return b__r[c__t];
  }

  var b__r = [1, 1];
  return a__e;
});
```
