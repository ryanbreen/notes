# You Won’t Believe This One Weird Way to Rewrite Closures
* Jonathan Martin
* (There was a goto meeting about this)

# Languages without closures
* Fortran (example) (no Clusures)
* x86 Assembly (no closures)
* 1995 -> Ruby (no closures)

"Convert a method to a lambd in Ruby: lambda(&method(:events_path)) or just use javascript" (@designer)

# Replacing closures
* "This stay-at-home develpoer built his own closures!"

## Do these 5 things every line:
* No function params, var, variable assignment
  * params are really just local variables
* No memoization
  * no attaching data to functions
* One global variable is allowed
* Amnesic function bodies allowed
  * don't remember anything about their scope (buckets of statements)
* shortcut local variables allowed

## Use this code, never run slow #JS again!

```javascript
s('local', 'Variable')
console.log(g('local'))
f('func', ['arg1', 'arg2', 'arg3'], function () {
    ...
});
```

## Your code can look...
```javascript
var outer = function (x, y) {
    var inner = function (x, z) {
        return [x, y, z];
    };
    return inner;
};

console.log(outer(1,2)(3,4))
```

Replaced With (something I didn't have time to write down)

## Coding Example

### Writing tests first, then writing out code (tests in mocha)
* get and set for parameters
* push & pop for maintaining scope
  * push adds things into scope, but if you write to them they won't affect the original value
  * pop removes all the local scope vars, returning the old scope
* Allow for nested IIFEs
* should remember scope when closure is invoked from outside

### Code

#### ClosureRegistry
```javascript
var ClosureRegistry = (function () {
    function ClosurRegistry() {
        this._closure = {};
        this._registry - [];
    }

    delegate(['push', 'pop'], { // adds methods to a prototye (from) that just delegate to another place (closure)
        from: ClosureRegistry.prototype,
        to: function () { return this._registry; }
    });

    delegate(['get', 'set', 'args'], {
        from: ClosureRegistry.prototype
        to: function () { return this.scopeForCurrentClosure(); }
    });

    Scope.prototype.push = function () {
        this._dict = this._forkDict();
    };

    Scope.prototype.pop = function () {
        this._dict = this._dict.__parent;
    };


    ClosureRegistry.prototype.scopeForCurrentClosure = function () {
        this._registry[this._registry.length - 1];
    };

    ClosureRegistry.prototpye.func = function (name, params, body /* function body */) {
        var _this = this;
        var scope = this.scopeForCurrentClosure().fork();
        this.set(name, function() {
            _this.push();
            _this.args(params, arguments);
            var result = body();
            _this.pop();
            return result;
        });
    };

    return ClosureRegistry;
}());
```

#### Scope
```javascript
var Scope = (function() {
    function Scope(dict) {
        this._dict = dict || {};
    }

    Scope.prototype.get = function (key) {
        return this._dict[key];
    };

    Scope.prototype.set = function (key, value) {
        this._dict[key]; = value;
    };

    Scope.prototype._forkDict = function () {
        var dict = this._dict;
        var F = function () {};
        var F.prototype = Object.create(dict);
        F.prototype.constructor = F;
        F.prototype.__parent = dict;
        return new F();
    };

    Scope.prototype.fork = function () {
        return new Scope(this._forkDict());
    };

    Scope.prototype.args = function (names, values) {
        for (var i = 0; i < names.length; i++) {
            this.set(names[i], values[i]);
        }
    };

    return Scope;
}());
```

## How can I use (this much more performant version of closures)?
* github.com/nybblr/closures
* Guarunteed job security if you use your own closure implementation


