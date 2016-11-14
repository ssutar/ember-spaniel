# ember-spaniel

Ember addon wrapping [spaniel](https://github.com/linkedin/spaniel), a viewport tracking library, [IntersectionObserver](https://github.com/WICG/IntersectionObserver) polyfill, and `requestAnimationFrame` task utility.

Including this addon will add Spaniel to your application, available for direct use in the app.

```JavaScript
import spaniel from 'spaniel';
```

The rest of the API is contained in a service.

## `viewport` service API

The `viewport` service will look for a `defaultRootMargin` object property on the application config. If not found, will default to 0, 0, 0, 0.

```JavaScript
// environment.js
module.exports = {
  ...
  defaultRootMargin: {
    top: 100,
    bottom: 200
  }
}
```

#### `onInViewportOnce(el, callback, { context, rootMargin, ratio })` => `Function`

Register a callback that will be called when the provided element first enters the viewport. Will get called on the next `requestAnimationFrame` if the element is already in the viewport. Returns a function that, when called, will cancel and clear the callback.

```JavaScript
export default Ember.Component.extend({
  viewport: Ember.inject.service(),
  didInsertElement() {
    let viewport = this.get('viewport');
    let el = this.get('element');
    this.clearViewportCallback = viewport.onInViewportOnce(el, () => {
      console.log('I am in the viewport');
    });
  },
  willDestroyElement() {
    this.clearViewportCallback();
  }
});
```

#### Global `Watcher`

The service has a `Watcher` instance available for direct use.

```JavaScript
export default Ember.Component.extend({
  viewport: Ember.inject.service(),
  didInsertElement() {
    let { watcher } = this.get('viewport');
    let el = this.get('element');
    watcher.watch(el, (e) => {
      console.log(`${e} happened`);
    });
  }
});
```

## Installation

* `git clone <repository-url>` this repository
* `cd ember-spaniel`
* `npm install`
* `bower install`

## Running

* `ember serve`
* Visit your app at [http://localhost:4200](http://localhost:4200).

## Running Tests

* `npm test` (Runs `ember try:each` to test your addon against multiple Ember versions)
* `ember test`
* `ember test --server`

## Building

* `ember build`

For more information on using ember-cli, visit [http://ember-cli.com/](http://ember-cli.com/).