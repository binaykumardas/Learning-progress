👉 If both exist:
setTimeout
Promise.then
# Which one executes first and why?
- lets take one example:-
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");

  - JavaScript first executes all synchronous code.
      * So Start is logged.
  - When setTimeout is encountered, its callback is delegated to the Web API, where the timer runs.
  - When Promise.resolve() is encountered, it resolves immediately and its .then() callback is placed into the microtask queue.
  - Then End is logged because it's synchronous.
  - At this point, the call stack becomes empty.
  - The Event Loop first processes the microtask queue completely before moving to the macrotask queue.
  - So Promise executes first.
  - After all microtasks are finished, the event loop takes the setTimeout callback from the macrotask queue and executes it.
  * Hence the final order:
      * Start → End → Promise → Timeout.