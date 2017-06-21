Specification
=============

**This document details an open specification for [node modules](https://nodejs.org) implementing [multi-armed bandit optimization algorithms](https://en.wikipedia.org/wiki/Multi-armed_bandit) via a minimal yet flexible interface.**


## API

### `Algorithm(config = {})`

The module must export a constructor function. The constructor must accept a `config` argument containing key-value pairs to use while instantiating the algorithm. The algorithm must accept a valid `state` object (e.g. as generated by `Algorithm#serialize`) as `config`. The constructor must synchronously return an instance of the algorithm.

The algorithm may provide default values for required `config` parameters. If a required parameter is missing from `config` or if a parameter is invalid (e.g. outside an expected range), a **synchronous** error should be thrown.

```js
/**
 * Create an instance of the multi-armed bandit optimization algorithm.
 *
 * @constructor
 * @param {Object} config Key-value pairs for algorithm setup or state restoration
 * @returns {Object} algorithm Optimizer instance.
 */
function Algorithm (config) {
  ...
}

module.exports = Algorithm;
```

### `Algorithm#select([params = {}])`

Execute the implemented algorithm to determine which arm should be played next. This method returns a promise that resolves to the index of the chosen arm. The returned promise may be rejected with an error. A rejection must occur if the current state is invalid.

```js
/**
 * Select an arm using the implemented algorithm.
 *
 * @async
 * @instance
 * @param {Object} [params] Key-value pairs for external context.
 * @returns {Number} arm The integer index of the selected arm.
 */
this.select (params) {
  return new Promise(function (resolve, reject) {
    ...
    resolve(arm);
  });
}
```

### `Algorithm#reward(arm, reward)`

Update the implemented algorithm with the result of an observed round. This method returns a promise that resolves the updated algorithm including any internal state updates made during reward processing. The returned promise may be rejected with an error.

```js
/**
 * Update the implemented algorithm with an observed reward.
 *
 * @async
 * @instance
 * @param {Number} arm The integer index of the selected arm.
 * @param {Number} reward The payoff received by playing the arm.
 * @returns {Object} algorithm Updated instance of the invoked algorithm.
 */
this.reward (arm, reward) {
  var self = this;

  return new Promise(function (resolve, reject) {
    ...
    resolve(self);
  });
}
```

### `Algorithm#serialize()`

Convert algorithm state into a plain object for external storage. This method returns a promise that resolves to the serialized state. The returned promise may be rejected with an error.

```js
/**
 * Serialize algorithm state into plain object.
 *
 * @async
 * @instance
 * @returns {Object} state The serialized state of the implemented algorithm.
 */
this.serialize () {
  return new Promise(function (resolve, reject) {
    ...
    resolve(state);
  });
}
```


## Versions

This specification uses [semantic versioning](http://semver.org). The target API may change as implemented algorithms introduce common requirements. Breaking and backward-incompatible changes will be introduced only after sincere debate and discussion among implementers.

**Changelog**

| Version | Description     | Status |
| :------ | :-------------- | :----- |
| 1.0.0   | Clarifications to specification examples | Final |
| 1.0.1   | Clarifications to specification examples | Final |
| 2.0.0   | `Algorithm#reward` resolves to `this`; `Algorithm` constructor supports `state` | Final |


## Conforming implementations

Please see the [implementations](IMPLEMENTATIONS.md) list.
