# Waitable Pattern

The `Waitable` and `Waiter` classes provide a pattern for wrapping long-running async processes (like blockchain transactions) with an event-based interface, while still maintaining standard Promise compatibility.

## Waiter<T>

A wrapper around an `EventEmitter` that manages the lifecycle of a promise.

### Methods

- `call(prom: Promise<T>)`: Connects a promise to the waiter.
- `on(eventName: string, callback: (arg: any) => void)`: Registers an event listener.
- `done(callback: () => void)`: Helper to listen for the `done` event.
- `catch(callback: (err: Error) => void)`: Helper to listen for the `error` event.

## Waitable<T>

The main class used to initiate a waitable process.

### Usage Example

```typescript
import { Waitable, Waiter } from '@elacity-js/common';

const myProcess = new Waitable<string>(async (waiter: Waiter<string>) => {
  // Do some work...
  waiter.emit('status', 'Started');
  
  await someAsyncWork();
  
  waiter.emit('status', 'Halfway there');
  
  return "Completed!";
});

// Standard Promise usage
const result = await myProcess.promise();

// Event-based usage
myProcess.wait()
  .on('status', (msg) => console.log(`Status: ${msg}`))
  .done(() => console.log('All done!'))
  .catch((err) => console.error(err));
```

### API Reference

#### `constructor(executor: (w: Waiter<T>) => Promise<T>)`
Initialize with a function that receives a `Waiter` and returns a `Promise`.

#### `promise(swallowError?: boolean): Promise<T>`
Returns the underlying promise. If `swallowError` is true, errors will be caught and logged as warnings instead of rejecting.

#### `wait(): Waiter<T>`
Returns the `Waiter` instance to register event listeners.
