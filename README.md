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

You can also call `tracery.setRandom` to use a random function other than `Math.random`.

    var tracery = require('tracery-grammar');
    var seedrandom = require('seedrandom');
    tracery.setRandom(seedrandom('controlled-seed'));

In this example, now all of the random calculations will use that seed, and repeatable results are possible.

You could also use a non-random function there if it suited your purposes. e.g.

    tracery.setRandom(() => 0.1)
    tracery.createGrammar({origin: ['#animal#'], animal: ['dog', 'cat']}).flatten('#origin#')

The result of that last line will always be 'cat' now.
