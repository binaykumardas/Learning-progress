# What is the Call Stack?
- The Call Stack in JavaScript is a data structure that keeps track of function execution.
Since JavaScript is single-threaded, it can execute only one function at a time, and the call stack manages thisexecution  order.

- Whenever a function is invoked, a new execution context is created and pushed onto the stack.
When that function finishes execution, it is popped off the stack.

- It follows the LIFO principle — Last In, First Out.
If the stack grows too large due to uncontrolled recursion, it results in a "Maximum call stack size exceeded" error.

- The call stack only handles synchronous code. Asynchronous operations like setTimeout or Promises are handled outsidethe   stack and are pushed back later via the event loop.

# What are the 3 types of execution contexts in JavaScript?
1️⃣ Global Execution Context (GEC)
   - Created when the JavaScript file first runs.
   - There is only one global execution context.
   - Stays in the call stack until program finishes.
2️⃣ Function Execution Context (FEC)
   - Created every time a function is invoked.
   - Each function call gets its own execution context.
   - Pushed to the call stack.
   - Removed after function finishes.
3️⃣ Eval Execution Context (Rare / Advanced)
   - Created when code runs inside eval().
   - Very rarely used in real production.
   - Avoided due to security and performance issues.

# if JavaScript is single-threaded and everything runs in call stack,
- Then how does setTimeout work?
- Where does it go?
- Who handles it?
- When does it come back?
    * When setTimeout is encountered, it is first pushed to the call stack like any other function.
    The browser provides Web APIs, and setTimeout is delegated to the Web API environment.
    The timer runs there independently.
    * Once the timer completes, its callback is moved to the macrotask queue.
    * The Event Loop continuously checks:
        - Is the call stack empty?
    * If the call stack is empty, the event loop pushes the callback from the macrotask queue into the call stack.
    Then the callback executes.