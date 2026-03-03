# Explain Angular change detection in depth.
Angular uses a tree-based change detection mechanism that performs dirty checking of bindings. It runs whenever Zone.js detects async operations like HTTP calls or events. By default, Angular checks the entire component tree, but with OnPush strategy, it only checks when input references change, events occur, or observables emit. For performance optimization in large applications, I prefer using OnPush with immutable data patterns.

 1️⃣ What is Change Detection?
 - Change Detection is the mechanism Angular uses to:
    - Detect changes in application state and update the DOM accordingly.
 - Whenever data changes → Angular updates the UI.

 2️⃣ How It Works Internally?
* Angular uses a tree of components.
    - Each component has:
    - A change detector
    - A view
When change detection runs:
 - Angular starts from root
 - Traverses component tree
 - Checks bindings ({{ }}, [property])
 - Compares previous value with new value
 - If different → updates DOM
 - This is called dirty checking.

3️⃣ What Triggers Change Detection?
Angular automatically runs change detection when:
 - User events (click, input)
 - HTTP response
 - setTimeout / setInterval
 - Promise resolution
 - Observable emits
 - Any async task

4️⃣ Role of Zone.js
Zone.js patches async APIs like:
    - setTimeout
    - Promise
    - addEventListener
    - XMLHttpRequest

When any async operation completes:
    - Zone.js notifies Angular
    - Angular runs change detection

 # What happens if Zone.js is removed?
 Angular’s change detection engine still exists, but it won’t auto-trigger after async operations. Zone.js patches async APIs and notifies Angular to run change detection. Without it, developers must manually trigger change detection using ChangeDetectorRef or rely on reactive patterns like signals or async pipe

 # When parent is Default and child is OnPush, when does child re-render?
 An OnPush child component only re-renders when its input reference changes, when an event originates from inside the component, when an observable bound via async pipe emits, or when change detection is manually triggered. It will not re-render just because the parent runs change detection unless the input reference changes.
