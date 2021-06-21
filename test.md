# JavaScript Interview Questions & Answers

1. ### What is a first class function

    In Javascript, functions are first class objects. First-class functions means when functions in that language are treated like any other variable.

    For example, in such a language, a function can be passed as an argument to other functions, can be returned by another function and can be assigned as a value to a variable.

2. ### What is a first order function

    First-order function is a function that doesn’t accept another function as an argument and doesn’t return a function as its return value.

3. ### What is a pure function

    A **Pure function** is a function where the return value is only determined by its arguments without any side effects. i.e, If you call a function with the same arguments 'n' number of times and 'n' number of places in the application then it will always return the same value.

4. ### What is the difference between let and var

    You can list out the differences in a tabular format

    | var | let |
    |---- | ---------
    | It is been available from the beginning of JavaScript  | Introduced as part of ES6 |
    | It has function scope | It has block scope  |
    | Variables will be hoisted | Hoisted but not initialized |

5. ### What is the reason to choose the name let as a keyword

    `let` is a mathematical statement that was adopted by early programming languages like **Scheme** and **Basic**. It has been borrowed from dozens of other languages that use `let` already as a traditional keyword as close to `var` as possible.

6. ### What is IIFE(Immediately Invoked Function Expression)

    IIFE (Immediately Invoked Function Expression) is a JavaScript function that runs as soon as it is defined. The signature of it would be as below,

7. ### What is the benefit of using modules

    There are a lot of benefits to using modules in favour of a sprawling. Some of the benefits are,
    1. Maintainability
    2. Reusability
    3. Namespacing

8. ### What are classes in ES6

    In ES6, Javascript classes are primarily syntactic sugar over JavaScript’s existing prototype-based inheritance.
    For example, the prototype based inheritance written in function expression as below,

9. ### What are closures

    A closure is the combination of a function and the lexical environment within which that function was declared. i.e, It is an inner function that has access to the outer or enclosing function’s variables. The closure has three scope chains
    
    1. Own scope where variables defined between its curly brackets
    2. Outer function’s variables
    3. Global variables

10. ### What are modules

    Modules refer to small units of independent, reusable code and also act as the foundation of many JavaScript design patterns.  Most of the JavaScript modules export an object literal, a function, or a constructor

11. ### Why do you need modules

    Below are the list of benefits using modules in javascript ecosystem
    1. Maintainability
    2. Reusability
    3. Namespacing

12. ### What is scope in javascript

    Scope is the accessibility of variables, functions, and objects in some particular part of your code during runtime. In other words, scope determines the visibility of variables and other resources in areas of your code.

13. ### What is a service worker

    A Service worker is basically a script (JavaScript file) that runs in the background, separate from a web page and provides features that don't need a web page or user interaction. Some of the major features of service workers are Rich offline experiences(offline first web application development), periodic background syncs, push notifications, intercept and handle network requests and programmatically managing a cache of responses.

14. ### How do you manipulate DOM using a service worker

    Service worker can't access the DOM directly. But it can communicate with the pages it controls by responding to messages sent via the `postMessage` interface, and those pages can manipulate the DOM.

15. ### What is IndexedDB

    IndexedDB is a low-level API for client-side storage of larger amounts of structured data, including files/blobs. This API uses indexes to enable high-performance searches of this data.

16. ### What is web storage

    Web storage is an API that provides a mechanism by which browsers can store key/value pairs locally within the user's browser, in a much more intuitive fashion than using cookies. The web storage provides two mechanisms for storing data on the client.
    1. **Local storage:** It stores data for current origin with no expiration date.
    2. **Session storage:** It stores data for one session and the data is lost when the browser tab is closed.

17. ### What is a Cookie

    A cookie is a piece of data that is stored on your computer to be accessed by your browser. Cookies are saved as key/value pairs.
    For example, you can create a cookie named username as below,

18. ### Why do you need a Cookie

    Cookies are used to remember information about the user profile(such as username). It basically involves two steps,
    1. When a user visits a web page, the user profile can be stored in a cookie.
    2. Next time the user visits the page, the cookie remembers the user profile.

19. ### How do you delete a cookie

    You can delete a cookie by setting the expiry date as a passed date. You don't need to specify a cookie value in this case.
    For example, you can delete a username cookie in the current page as below.

20. ### What are the differences between cookie, local storage and session storage

    Below are some of the differences between cookie, local storage and session storage,

    | Feature | Cookie | Local storage | Session storage |
    |---- | --------- | ----- | ----- |
    | Accessed on client or server side | Both server-side & client-side | client-side only | client-side only |
    | Lifetime | As configured using Expires option  | until deleted | until tab is closed |
    | SSL support | Supported | Not supported | Not supported |
    | Maximum data size | 4KB | 5 MB | 5MB |

21. ### What is the main difference between localStorage and sessionStorage

    LocalStorage is the same as SessionStorage but it persists the data even when the browser is closed and reopened(i.e it has no expiration time) whereas in sessionStorage data gets cleared when the page session ends.

22. ### How do you access web storage

    The Window object implements the `WindowLocalStorage` and `WindowSessionStorage` objects which has `localStorage`(window.localStorage) and `sessionStorage`(window.sessionStorage) properties respectively. These properties create an instance of the Storage object, through which data items can be set, retrieved and removed for a specific domain and storage type (session or local).
    For example, you can read and write on local storage objects as below

23. ### What is a storage event and its event handler

    The StorageEvent is an event that fires when a storage area has been changed in the context of another document. Whereas onstorage property is an EventHandler for processing storage events.

24. ### Why do you need web storage

    Web storage is more secure, and large amounts of data can be stored locally, without affecting website performance. Also, the information is never transferred to the server. Hence this is a more recommended approach than Cookies.

25. ### What is a promise

    A promise is an object that may produce a single value some time in the future with either a resolved value or a reason that it’s not resolved(for example, network error). It will be in one of the 3 possible states: fulfilled, rejected, or pending.

26. ### What is a callback function

    A callback function is a function passed into another function as an argument. This function is invoked inside the outer function to complete an action.

27. ### Why do we need callbacks

    The callbacks are needed because javascript is an event driven language. That means instead of waiting for a response javascript will keep executing while listening for other events.

28. ### What are events

     Events are "things" that happen to HTML elements. When JavaScript is used in HTML pages, JavaScript can `react` on these events. Some of the examples of HTML events are,

     1. Web page has finished loading
     2. Input field was changed
     3. Button was clicked

29. ### What are the various url properties of location object

     The below `Location` object properties can be used to access URL components of the page,
     1. href - The entire URL
     2. protocol - The protocol of the URL
     3. host - The hostname and port of the URL
     4. hostname - The hostname of the URL
     5. port - The port number in the URL
     6. pathname - The path name of the URL
     7. search - The query portion of the URL
     8. hash - The anchor portion of the URL

## Vue js topics

### Config
* silent
* optionMergeStrategies
* devtools
* errorHandler
* ignoredElements
* keyCodes
* performance
* productionTip

### Lifecycle Hooks
* beforeCreate
* created
* beforeMount
* mounted
* beforeUpdate
* updated
* activated
* deactivated
* beforeDestroy
* destroyed

### DOM Keywords
* el
* template
* render
* renderError

### Composition
* parent
* mixins
* extends
* provide / inject

### Events
* vm.$on
* vm.$once
* vm.$off
* vm.$emit

### Properties
* vm.$data
* vm.$props
* vm.$el
* vm.$options
* vm.$parent
* vm.$root
* vm.$children
* vm.$slots
* vm.$scopedSlots
* vm.$refs
* vm.$isServer


### Directives HTML
* v-text
* v-html
* v-show
* v-if
* v-else
* v-else-if
* v-for
* v-on
* v-bind
* v-model
* v-pre
* v-cloak
* v-once

### Built-In Components
* component
* transition
* transition-group
* keep-alive
* slot

### Event Modifiers
* v-on:click .native
* v-on:click .stop
* v-on:click .prevent
* v-on:click .passive
* v-on:click .capture
* v-on:click .self
* v-on:click .once

![image](https://assets.digitalocean.com/articles/alligator/vuejs/external/component-lifecycle.png)

Good luck with your interview 😊

---
