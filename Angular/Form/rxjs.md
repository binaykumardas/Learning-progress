* You have a dashboard page with:
    - 3 API calls
    - getUser()
    - getPermissions()
    - getNotifications()
    - Requirements:
1. All 3 should run in parallel
2. If any one fails → show error page
3. Loader should show until all complete
4. UI should render only after all responses come

👉 How would you implement this properly using RxJS?
Explain:
  - Which operator you will use
  - Why not combineLatest?
  - How you handle error?
  - How you handle loader?
* Answer
3 API calls
    - getUser()
    - getPermissions()
    - getNotifications()
 - Run in parallel
 - Show loader until all complete
 - If any one fails → show error page
 - Render UI only after all success

 ✅ Correct Operator: forkJoin
 Because:
    - Runs observables in parallel
    - Waits until ALL complete
    - Emits once with all results
    - If ANY fails → throws error
    - Perfect for page initialization.

🔥 Why NOT combineLatest?
| combineLatest           | forkJoin                 |
| ----------------------- | ------------------------ |
| Emits when any emits    | Emits once               |
| Continuous stream       | One-time API calls       |
| Needs all to emit first | Waits until all complete |


1. How would you handle multiple API calls on page load?
For parallel one-time API calls, I use forkJoin because it waits until all observables complete and emits once. I handle loader using finalize() and manage errors inside subscribe or using catchError depending on whether the page should fail completely or partially render

2. If API 2 depends on API 1 result, which operator do you use?
In most UI-driven scenarios, I use switchMap because it cancels previous requests and prevents race conditions. If order must be strictly preserved and cancellation is not desired, I use concatMap.

3. What operator would you use to prevent multiple rapid button clicks?
I would use exhaustMap because it ignores new emissions while the current observable is active. This prevents duplicate API calls without cancelling the existing request.

| Operator   | Cancels Previous | Queues | Ignores New | Parallel |
| ---------- | ---------------- | ------ | ----------- | -------- |
| switchMap  | ✅ Yes            | ❌ No   | ❌ No        | ❌ No     |
| concatMap  | ❌ No             | ✅ Yes  | ❌ No        | ❌ No     |
| mergeMap   | ❌ No             | ❌ No   | ❌ No        | ✅ Yes    |
| exhaustMap | ❌ No             | ❌ No   | ✅ Yes       | ❌ No     |

4. How would you implement a search box using RxJS?
I would use debounceTime to limit rapid typing, distinctUntilChanged to avoid duplicate requests, and switchMap to cancel previous API calls and prevent race conditions.

5. What happens if search API fails?
If error is not handled inside switchMap, the entire observable chain will terminate. To prevent this, I use catchError inside the inner observable and return a fallback value like of([]), ensuring the search stream continues to function

6. How do you prevent API calls from continuing after navigation?
I ensure all subscriptions are properly cleaned up using takeUntil or Angular’s takeUntilDestroyed. If possible, I prefer async pipe which handles unsubscription automatically. This prevents memory leaks and cancels in-flight HTTP requests when the component is destroyed.

