# What is async validator?
- An Async Validator in Angular is a function that validates form data asynchronously (using API calls, database checks, etc.) and returns: Promise<ValidationErrors | null> or Observable<ValidationErrors | null>.

# Difference between Sync Validator & Async Validator?
| Sync Validator               | Async Validator             |                                   |
| ---------------------------- | --------------------------- | --------------------------------- |
| Runs immediately             | Runs after sync validators  |                                   |
| Returns `ValidationErrors    | null`                       | Returns `Promise` or `Observable` |
| Example: required, minLength | Example: email exists check |                                   |

# Why we need async validator?
We use Async Validators because the browser does not know everything.
Standard (Synchronous) validators check things the browser can see immediately:
* "Is the field empty?" (Required)
* "Are there 5 characters?" (MinLength)
* "Does it look like an email?" (Regex)
Async Validators are used to answer questions that only the Server/Database knows.

- To Check Uniqueness (The #1 Reason)
    - Imagine you have 1,000,000 users in your database. You cannot download that entire list to the user's browser to check if   "john_doe" is taken (it would be slow and a security risk).
    * The Problem: The browser has no idea if "john_doe" is already registered.
    * The Solution: An Async Validator sends "john_doe" to the server. The server checks the database and replies "Yes, it's taken" or "No, it's free."

- To Verify Existence (External APIs)
    Sometimes you need to check if a specific code represents a real thing in the real world.
    * Zip/Pin Codes: A user enters "90210". An async validator calls a postal API to see if that Zip Code actually exists and   matches the selected city.
    * Coupon Codes: A user enters "SUMMER2024". The browser doesn't know if that code is valid or expired. You must ask the     backend billing system.
    * Referral IDs: "Enter the ID of the friend who invited you." You need to check if that ID actually belongs to a real user.

- Better User Experience (Don't wait for Submit)
    Without Async Validators, the flow looks like this (The "Old Way"):
    1. User fills out a long form (Name, Email, Password, Address...).
    2. User clicks SUBMIT.
    3. User waits...
    4. Server returns an error: "Email already exists."
    5. User gets frustrated, has to change the email, and submit again.
    With Async Validators:
    1. User types email.
    2. IMMEDIATELY (or after 500ms) a red text appears: "Email already taken".
    3. User fixes it before even touching the Submit button.
    4. The User is happy.
* Summary
We use Async Validators when the answer to "Is this valid?" lives on a Server, not in the Browser.
