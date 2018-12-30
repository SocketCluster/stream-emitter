# AsyncStreamEmitter
EventEmitter using AsyncIterableStream.

## Methods:

- emit(eventName, data)
- listener(eventName)
- closeListener(eventName)
- closeAllListeners()

## Usage examples

```js
let emitter = new AsyncStreamEmitter();

(async () => {
  await wait(10);
  emitter.emit('foo', 123, 'hello');

  // This will cause all for-await-of loops for that event to exit.
  // Note that you can also use the 'break' statement inside
  // individual for-await-of loops.
  emitter.closeListener('foo');
})();

(async () => {
  // The event data is always an array so we should use destructuring.
  for await (let [someNumber, someString] of emitter.listener('foo')) {
    // someNumber is 123
    // someString is 'hello'
  }
  console.log('The listener was closed');
})();

// Utility function.
function wait(duration) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve();
    }, duration);
  });
}
```

Note that unlike with `EventEmitter`, you cannot get the count for the number of active listeners at any given time.
This is intentional as it encourages code to be written in a more declarative style.

If you want to track listeners, you should do it yourself.
The new ECMAScript `Symbol` type should make tracking object references easier: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol
