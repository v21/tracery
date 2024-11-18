# tracery
Tracery: a story-grammar generation library for javascript

This is my attempt to package up [Tracery](https://github.com/galaxykate/tracery/) as a Node library.

## Installation

This is hosted at npm, so it can be installed like so:

```bash
$ npm install tracery-grammar --save
```

## Example usage

```javascript
var tracery = require('tracery-grammar');

var grammar = tracery.createGrammar({
  'animal': ['panda','fox','capybara','iguana'],
  'emotion': ['sad','happy','angry','jealous'],
  'origin':['I am #emotion.a# #animal#.'],
});

grammar.addModifiers(tracery.baseEngModifiers); 

console.log(grammar.flatten('#origin#'));
```

Sample output:

```plaintext
I am a happy iguana.
I am an angry fox.
I am a sad capybara.
```

### Making Tracery deterministic

By default, Tracery uses Math.random() to generate random numbers. If you need Tracery to be deterministic, you can make it use your own random number generator using:

    tracery.setRng(myRng);

where myRng is a function that, [like Math.random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random), returns a floating-point, pseudo-random number in the range [0, 1).

By using a local random number generator that takes a seed and controlling this seed, you can make Tracery's behavior completely deterministic.


```javascript
// Stable random number generator
// Copied from this excellent answer on Stack Overflow: https://stackoverflow.com/a/47593316/3306
function splitmix32(seed) {
  return function() {
    seed |= 0; // bitwise OR ensures this is treated as an integer internally for performance.
    seed = seed + 0x9e3779b9 | 0; // again, bitwise OR for performance 
    let t = seed ^ seed >>> 16;
    t = Math.imul(t, 0x21f0aaad);
    t = t ^ t >>> 15;
    t = Math.imul(t, 0x735a2d97);
    return ((t = t ^ t >>> 15) >>> 0) / 4294967296;
  };
}

var seed = 123456;
tracery.setRng(splitmix32(seed));

console.log(grammar.flatten('#origin#'));
```

Deterministic output:

```plaintext
I am an angry capybara.
```