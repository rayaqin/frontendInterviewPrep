# General Frontend knowledge + React

- [General Frontend knowledge + React](#general-frontend-knowledge--react)
  - [**Function generators in JS**](#function-generators-in-js)
  - [**Higher order component in React**](#higher-order-component-in-react)
  - [**Higher order function**](#higher-order-function)
  - [**Async keyword**](#async-keyword)
  - [**Pure function, PureComponent**](#pure-function-purecomponent)
  - [**Difference between `for of` and `for in`**](#difference-between-for-of-and-for-in)
  - [**React hooks in loops and conditions**](#react-hooks-in-loops-and-conditions)
  - [**Redux middlewares saga and thunk**](#redux-middlewares-saga-and-thunk)
  - [**Acronyms of best practices, clean code**](#acronyms-of-best-practices-clean-code)
  - [**Some basic Design Patterns**](#some-basic-design-patterns)
    - [Constructor Pattern <br>](#constructor-pattern-)
    - [Module Pattern <br>](#module-pattern-)
    - [Singleton Pattern <br>](#singleton-pattern-)
    - [Observer Pattern <br>](#observer-pattern-)
  - [**React Fiber**](#react-fiber)
  - [**What is jank?**](#what-is-jank)
  - [**Real usecase for useMemo and useCallback**](#real-usecase-for-usememo-and-usecallback)
  - [**FirstClassFunction**](#firstclassfunction)
  - [**React.memo(), when to use it, it’s parameter function**](#reactmemo-when-to-use-it-its-parameter-function)
  - [**Reconciliation**](#reconciliation)
  - [**Currying**](#currying)
  - [**Ways to skip rendering in react**](#ways-to-skip-rendering-in-react)
  - [**Notes on Redux reducers, actions, dispatch**](#notes-on-redux-reducers-actions-dispatch)
  - [**3 main principles of Redux**](#3-main-principles-of-redux)
  - [**React's Context API**](#reacts-context-api)
  - [**Framework vs Library**](#framework-vs-library)
  - [**How does `window.requestAnimationFrame(callback)` work, and what is it for?**](#how-does-windowrequestanimationframecallback-work-and-what-is-it-for)
  - [**The Window object**](#the-window-object)
  - [**The History object**](#the-history-object)
  - [**3 Core Web Vitals**](#3-core-web-vitals)
    - [**LCP (Largest Contentful Paint)**](#lcp-largest-contentful-paint)
    - [**FID (First Input Delay)**](#fid-first-input-delay)
    - [**CLS (Cumulative Layout Shift)**](#cls-cumulative-layout-shift)
  - [**Microtasks and Macrotasks**](#microtasks-and-macrotasks)
  - [**How does the browser run JavaScript?**](#how-does-the-browser-run-javascript)
  - [**Promise.all(), Promise.allSettled, Promise.race()**](#promiseall-promiseallsettled-promiserace)
    - [**Promise.all():**](#promiseall)
    - [**Promise.allSettled():**](#promiseallsettled)
    - [**Promise.race():**](#promiserace)
  - [**stopPropagation/stopImmediatePropagation/preventDefault**](#stoppropagationstopimmediatepropagationpreventdefault)
  - [**.addEventListener()**](#addeventlistener)
  - [**useSelector hook**](#useselector-hook)
  - [**useDispatch hook**](#usedispatch-hook)
  - [**Render batching in react**](#render-batching-in-react)
  - [**Shadow DOM**](#shadow-dom)
  - [**Real life use for closures**](#real-life-use-for-closures)
  - [**JavaScript and programming paradigms**](#javascript-and-programming-paradigms)
  - [**What is functional programming?**](#what-is-functional-programming)
  - [**What is a side-effect in programming**](#what-is-a-side-effect-in-programming)
  - [**How to call async functions that only need to be run once in useEffect**](#how-to-call-async-functions-that-only-need-to-be-run-once-in-useeffect)
  - [**React.lazy and React.Suspense**](#reactlazy-and-reactsuspense)
  - [**Types vs Interfaces in TypeScript**](#types-vs-interfaces-in-typescript)
  - [**Generics in TypeScript**](#generics-in-typescript)
  - [**Page optimization methods**](#page-optimization-methods)
  - [**Virtual scrolling**](#virtual-scrolling)
  - [**useLayoutEffect()**](#uselayouteffect)
  - [**useDeferredValue()**](#usedeferredvalue)
  - [**useTransition()**](#usetransition)

## **Function generators in JS**
The function* declaration defines a generator function, which **returns a Generator object**. This object cannot be instantiated directly, a Generator instance can only be returned from a generator function. **Generators are functions that can be exited and later re-entered**. Their context (variable bindings) will be saved across re-entrances. **Calling a generator function does not execute its body immediately; an iterator object for the function is returned instead**.
When the iterator's `next()` method is called, the generator function's body is executed until the first `yield` expression, which specifies the value to be returned from the iterator.

The function can be called as many times as desired, and **returns a new Generator each time**. Each Generator **may only be iterated once**.
```
function* generator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = generator();

console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3
```
<br>

**Instance methods**<br>
`Generator.prototype.next()`<br>
Returns a value yielded by the yield expression.<br><br>
`Generator.prototype.return()`<br>
Returns the given value and finishes the generator.<br><br>
`Generator.prototype.throw()`<br>
Throws an error to a generator (also finishes the generator, unless caught from within that generator).
<br>

*Example: Redux Sagas are implemented as Generator functions that yield objects to the redux-saga middleware.*
<br>

**Iteration protocols**<br>
These protocols can be implemented by any object by following some conventions. These objects define or customize their iteration protocol in a way that allows their values to be looped over. **Array** and **Map** have default **build-in iteration behaviour**. In order to be iterable an object must implement the `@@iterator` method, meaning it must have a property with the `@@iterator` key (Symbol.interator). This is a zero argument function that returns an object. The returned `next()` method must return an object with `done` and `value` properties. 

```
const a = ['a', 'b', 'c'];
const iterator = a[Symbol.iterator]();
undefined
iterator.next();
{value: 'a', done: false}
iterator.next();
{value: 'b', done: false}
iterator.next();
{value: 'c', done: false}
iterator.next();
{value: undefined, done: true}
```

<br>
<br>

## **Higher order component in React**
A higher-order component is **a function that takes a component as parameter and returns a new component**.

<br>

**Example: Redux connect**<br>
The `connect()` function connects a React component to a Redux store *(kinda deprecated though)*.<br>
It provides its connected component with the pieces of the data it needs from the store, and the functions it can use to dispatch actions to the store.
It does not modify the component class passed to it; instead, it **returns a new, connected component class that wraps the component you passed in**.

<br>
<br>

## **Higher order function**
A higher order function accepts functions as parameters or returns a function.<br>
For example:
```
const isEven = (x) => (x%2 ===0);
[1,2,3,4,5].filter(isEven);
```

<br>
<br>

## **Async keyword**
An async function is a function declared with the `async` keyword, and the `await` keyword is permitted within it. The async and await keywords enable asynchronous, **promise-based behavior** to be written **in a cleaner style**, avoiding the need to explicitly configure promise chains. The resolved value of the promise is treated as the return value of the await expression. Use of async and await **enables the use of ordinary try / catch blocks** around asynchronous code.

**Async functions always return a promise**. If the return value of an async function is not explicitly a promise, it will be implicitly wrapped in a promise.<br>
For example, the following:

```
async function foo() {
   return 1
}
```
is similar to:
```
function foo() {
   return Promise.resolve(1)
}
```
and then you can:
```
  foo().then((r)=>console.log(r));
```

<br>
<br>

## **Pure function, PureComponent**
A **Pure Function always returns the same result if the same arguments are passed** *(deterministic)*.<br>
It does not depend on any outside state or data change during a program's execution. Rather, it **only depends on its input arguments**.
<br>

*Note: A side effect is when a function relies on, or modifies, something outside of its parameters.*

<br>

ReactJS's **PureComponent** Class always renders the same output for the same state and props values.<br>
To achieve this, it **compares current state and props with new props and states to decide whether the React component should re-render** itself.

<br>
<br>

## **Difference between `for of` and `for in`**
**for in -> iterates over keys in an object and indexes in an array**
```
let obj={a:1, b:2}
    
for (const key in obj) {
  console.log(key);      // a, b
  console.log(obj[key]); // 1, 2
}

let arr = [10, 11, 12, 13];

for (const item in arr) {
  console.log(item);   // 0 1 2 3
}
```
**for of -> iterates over values in an array or any iterable**
```
let arr = [10, 11, 12, 13];

for (const item of arr ) {
  console.log(item);  // 10  11  12  13
}
```

<br>
<br>

## **React hooks in loops and conditions**
**Don't call Hooks inside loops, conditions, or nested functions**. Instead, always **use Hooks at the top level** of your React function, **before any early returns**. By following this rule, you **ensure that Hooks are called in the same order each time a component renders**. 
Though function components may conditionally call setSomeState() directly while rendering, as long as it is NOT going to execute every time this component renders.

<br>
<br>

## **Redux middlewares saga and thunk**
Redux’s actions are dispatched synchronously, which may be a problem for a complex app that needs to communicate with an external API or perform side effects.<br>
Redux allows for middleware that sits between an action being dispatched and the action reaching the reducers.
There are two very popular middleware libraries that **allow for side effects and asynchronous actions**: Redux Thunk and Redux Saga.

<br>

Saga is like a separate thread in your application that's solely responsible for side effects.<br>
`redux-saga` is a redux middleware that **allows the creation of Sagas, which are threads can be started, paused and cancelled from the main application with normal redux actions**, and have access to the full redux application state and dispatch redux actions as well.<br>
Further reading: https://redux-saga.js.org/docs/introduction/BeginnerTutorial

<br>

Thunk is a programming concept where a function is used to delay the evaluation/calculation of an operation.<br>
`redux-thunk` is a redux middleware that lets you call action creators that return a function instead of an action object. This allows for **dispatching actions asyncronously**, including working with promises. This is commonly used for data fetching.<br>
Further reading: https://redux.js.org/usage/writing-logic-thunks


<br>
<br>

## **Acronyms of best practices, clean code**
DRY - don’t repeat yourself <br>
KISS - keep it simple + stupid<br>
YAGNI
"You aren't gonna need it" is a principle that states a programmer should not add functionality until deemed necessary.<br>
SOLID
* S - Single-responsiblity Principle.
* O - Open-closed Principle. Objects or entities should be open for extension but closed for modification.
* L - Liskov Substitution Principle. Every subclass or derived class should be substitutable for their base or parent class
* I - Interface Segregation Principle. A client should never be forced to implement or depend on an interface that it doesn’t use.
* D - Dependency Inversion Principle. A high-level module must not depend on the low-level module.
<br>
<br>

## **Some basic Design Patterns**
### Constructor Pattern <br>
When we think on the classic implementation of object oriented languages, a constructor is a special function that initializes the class’s values on default or with input from the caller.<br>
*Note: In JS, `Object.Create` builds an object that inherits directly from the one passed as its first argument and does not execute the constructor. If you want the constructor to be executed use the `new` keyword.*
<br>

### Module Pattern <br>
Access rights to make sure everything only has access to things that it should be able to modify.
<br>

### Singleton Pattern <br>
When we want to allow a single instance of a class, we use the singleton pattern.
<br>

### Observer Pattern <br>
An object named the **subject**, maintains a list of its dependents, called **observers**, and notifies them automatically of any state changes, usually by calling one of their methods. It is mainly used for implementing distributed event handling systems, in "event driven" software.

<br>
<br>

## **React Fiber**
The Fiber reconciler, which became the default reconciler for React 16 and above, is a complete rewrite of React’s reconciliation algorithm to solve some long-standing issues in React.<br>
Because Fiber is asynchronous, React can:
- **Pause, resume, and restart rendering** work on components as new updates come in
- Reuse previously completed work and even abort it if not needed
- **Split work** into chunks and **prioritize tasks**

A fiber represents a unit of work with its **own virtual stack** which can be kept in memory and executed however and whenever desired.<br>
React creates a **tree of fiber nodes** that can mutate, and stores them **in a singly-linked list**, which can be traversed by depth-first search *(the algorithm starts at the root node and explores as far as possible along each branch before backtracking)*.<br>
A fiber node effectively **holds the component’s state, props, and the underlying DOM elements**.<br>

This change allows React to break away from the limits of the synchronous stack reconciler. **Previously**, you could add or remove items, but the whole rendering process had to finish once started, and **tasks could not be interrupted**.

**Now React** effectively **runs an internal timer** for each unit of work being performed **and constantly monitors this time limit while performing the work**.<br>
**The moment the time runs out, React pauses the current unit of work**, hands the control back to the main thread, and lets the browser render whatever is finished at that point.<br>
**Then**, in the next frame, React **picks up where it left off** and continues building the tree. Then, when it has enough time, it completes the render.

<br>

As a result, React is slowly **moving away from** some old lifecycle hooks:<br>
`componentWillMount`, `componentWillUpdate`, `componentWillReceiveProps`<br>
can now fire multiple times. You shouldn't trigger side effects here anymore.<br><br>
Now, you want to fire side effects in the lifecycle hooks that will only fire once:<br>
`componentDidMount` and `componentDidUpdate`

<br>

To make up for a lot of the use cases that `componentWillReceiveProps` covered, two new hooks have been created:
- `getDerivedStateFromProps` <br>
  This is the method that is **invoked first when a component is being rendered**, right before calling the render method, both on the initial mount and on subsequent re-renders. It returns an object to update the state, or null. This method exists for **rare use cases where the state depends on changes in props over time**. Note that this method is **fired on every render**, regardless of the cause.

- `getSnapshotBeforeUpdate` <br>
  This method is invoked **right before the most recently rendered output is committed the DOM**. It enables your component to **capture** some information from the DOM (e.g. **scroll position**) **before it is** potentially **changed**. Any value returned by this lifecycle method will be passed as a parameter to `componentDidUpdate()`.

<br>

Further reading: https://blog.logrocket.com/deep-dive-react-fiber/#what-react-fiber
<br>
<br>

## **What is jank?**
Most devices these days refresh their screens at 60 FPS (1/60ms = 16.67ms), which means a new frame displays every 16ms. This number is important because if the React renderer takes more than 16ms to render something on the screen, the browser drops that frame.

In reality, however, the browser has housekeeping to do, so all your work must complete inside 10ms. When you fail to meet this budget, **the frame rate drops, and the content becomes shaky on screen**. This is often referred to as **jank**, and it negatively impacts the user’s experience.

<br>
<br>

## **Real usecase for useMemo and useCallback**
- useMemo is used to memoize and **skip recalculation**. It is useful when you don't want to recalculate heavy calculations each time a component renders *(usecase: when sorting or filtering large amounts of data)*
- useCallback is used to **avoid recreating/redefining methods at every render** *(when a function is being passed down to a React child component, and we want to prevent unnecessary rerender of the child, or when a function would be called repeatedly with the same params in a useEffect)*

- use eslint-plugin-react-hook to receive warnings about this

<br>
<br>

## **FirstClassFunction**
A programming language is said to have First-class functions when **functions** in that language **are treated like any other variable**. For example, in such a language, a function can be passed as an argument to other functions, can be returned by another function and can be assigned as a value to a variable.
Pretty much all functional languages *(like Haskell)*, and Perl, Python, PHP, Lua, Tcl/Tk, **JavaScript** have first class functions.


<br>
<br>

## **React.memo(), when to use it, it’s parameter function**
It is a built-in "higher order component" type. It accepts a component type as an argument, and returns a new wrapper component.
You gain a performance boost by **reusing the memoized content**, React **skips rendering** the component and doesn't perform a virtual DOM difference check.
The wrapper component's default behavior is to **check to see if any of the props have changed**, and if not, prevent a re-render. By default it will only shallowly compare complex objects in the props object. If you want control over the comparison, **you can also provide a custom comparison function** (areEqual) **as the second argument**. `React.memo` only checks for prop changes, but if your function component wrapped in `React.memo` has a useState, useReducer or useContext hook in its implementation, it will still re-render when state or context change.

Use it when the component:
- always renders the same output for the same input
- renders often
- it contains at least a decent amount of UI elements
Do not use it when:
- the component isn't heavy, and renders usually with different props.
- your component is a class-based one *(then extend PureComponent or use shouldComponentUpdate instead)*

<br>
<br>

## **Reconciliation**
React's heuristic **algorithm** for finding a way **to transform one component tree into another** after finding all the differences between them, starting from the root elements.
It relies on two assumptions:
- Two elements of **different types will produce different trees**.
- The developer can **hint at which child elements may be stable** across different renders **with a key prop**.

<br>

When you call `ReactDOM.render()` or `setState()`, React performs reconciliation.

<br>
<br>

## **Currying**
Currying is the technique of **converting a function that takes multiple arguments into a sequence of functions that each take a single argument**.
```
function curryThis(f) {
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}

function sum(a, b) {
  return a + b;
}

let curriedSum = curryThis(sum);

curriedSum(1)(2); // 3
```

<br>
<br>

## **Ways to skip rendering in react**
Skipping rendering a component means React will **also skip rendering that entire subtree**. It's effectively putting a stop sign up to halt the default "render children recursively" behavior.

`shouldComponentUpdate`, `PureComponent`, `React.memo()`, `useMemo`
<br>
<br>

## **Notes on Redux reducers, actions, dispatch**
In Redux, a reducer is a pure function that takes an action and the previous state of the application and returns the new state. The action describes what happened and it is the reducer's job to return the new state based on that action.
There's no such thing as dispatching to a certain reducer. Every time you dispatch an action, all reducer functions are executed. The only way to make sure the right piece of code is executed is to make sure the name of the action type is unique, and is present exactly in one reducer among all.

<br>
<br>

## **3 main principles of Redux**
- Single source of truth |​ *The global state of your application is stored in an object tree within a single store*
- State is read-only |​ *reducers don't modify the existing state, they return a new one*
- Changes are made with pure functions​

<br>
<br>

## **React's Context API**
The React Context API is designed for **"prop drilling"**.<br>
Context provides **a way to share values** *(such as the current authenticated user, theme, or preferred language)* **between components** without having to explicitly pass a prop through every level of the tree.<br>
Context is primarily used **when some data needs to be accessible by many components at different nesting levels**.
If you only want to avoid passing some props through many levels, component composition is often a simpler solution than context.<br>
It is **not for global state management**, though it can be used that way, it is not optimal to do so.
<br>

**Additional info on Redux vs Context:**<br>
Redux can be used independently from React<br>
Recommendation is to use *Redux* for complex **global state management** and *Context* for **prop drilling**.

<br>

Further reading: https://reactjs.org/docs/context.html#when-to-use-context <br>
*Note: Redux is not the only state management tool available.*

<br>
<br>

## **Framework vs Library**
Frameworks and libraries are **both** code written by someone else that **help**s **you perform some common tasks in a less verbose way**.
Generally **frameworks are more opinionated**, and limit what you can do and how you can do it, but provide more assistance and leave less to the developer, while the developer can decide when and how to use a library.
- A library is essentially a set of functions, classes and types. Each time you call a function of the library, it does some work and **returns control to the client**.
- A framework embodies some abstract design, with **more behavior built in**. In order to use it you need to insert your behavior into various places in the framework either by subclassing or by plugging in your own classes. The framework's code then **calls your code** at these points.

<br>
<br>

## **How does `window.requestAnimationFrame(callback)` work, and what is it for?**
It is used for **script-based animations**.
You should call this method whenever you're ready to update your animation onscreen. This will **request that your animation function be called before the browser performs the next repaint**. The number of callbacks is usually 60 times per second, but will generally match the display refresh rate in most web browsers as per W3C recommendation. `requestAnimationFrame` calls are **paused** in most browsers **when running in background** tabs or a hidden `<iframe>` in order to improve performance and battery life.

<br>

The callback method is the function to call when it's time to update your animation for the next repaint. The callback function is passed one single argument, a `DOMHighResTimeStamp` similar to the one returned by `performance.now()`, indicating the point in time when `requestAnimationFrame` starts to execute callback functions. <br>
It returns a long integer value, the request id, that uniquely identifies the entry in the callback list. This is a non-zero value, but you may not make any other assumptions about its value. You can pass this value to `window.cancelAnimationFrame()` to cancel the refresh callback request. <br>

It does not guarantee synchronicity of two animations.

**Old-school methods before requestAnimationFrame:** <br>
`setTimeout` and `setInterval` <br>
JavaScript timers continue to work even in background tabs, and even when the corresponding browser window is minimized. As a consequence, the browser will continue to run invisible animations, resulting in unnecessary CPU usage and wastage of battery life. This is especially bad in the case of mobile devices. <br>
Additionally, timers set up this way always enqueue their callback functions after a given time, even if for some reason the execution of the callback function takes more time, and this may result in multiple callback functions running at the same time, which can choke up the browser or result in unexpected behaviour. <br>
Plus if the redraws are not synced with the display refresh rate, the animation might be choppy, or CPU usage and battery consumption might be higher than necessary.

Futher reading: https://dev.opera.com/articles/better-performance-with-requestanimationframe/, https://humanwhocodes.com/blog/2011/05/03/better-javascript-animations-with-requestanimationframe/


<br>
<br>

## **The Window object**
The global variable: `window` represents the window in which the script is running, and is exposed to JavaScript code.<br>
The **Window interface** is home to a variety of functions, namespaces, objects, and constructors which are not necessarily directly associated with the concept of a user interface window. However, the Window interface is **a suitable place to include these items that need to be globally available** *(though should be used this way sparingly)*.<br>
In a tabbed browser, **each tab is represented by its own Window object**.
<br><br>
Some example properties:
- `Window.history` *- returns a reference to the history object*
- `Window.document` *- returns a reference to the document that the window contains*
- `Window.location` *- can be used to get/set the location, or current URL, of the window object*
- `Window.fullScreen` *- property indicates whether the window is displayed in full screen or not*
- `Window.screen` *- returns reference to the screen object associated with the window*

Some example methods:
- `Window.close()`
- `Window.blur()`
- `Window.scroll()`
<br>
<br>

## **The History object**
The `Window` object provides access to the browser's **session history** through the history object.
It exposes useful **methods and properties that let you navigate back and forth through the user's history**, and **manipulate the contents of the history stack**.<br>
Moving backward and forward through the user's history is done using these methods:
- `back()` *- acts as if the user clicked on the back button*
- `forward()` *- acts as if the user clicked on the forward button*
- `go()` *- to load a specific page from session history, identified by its relative position to the current page* 
- `pushState(state, unused, url)` *- adds an entry to the browser's session history stack*

*Note: The `popstate` **event** of the Window interface is **fired when** the active **history entry changes** while the user navigates the session history (to the new state). One can react to this event by modifying the value of `window.onpopstate`.*
<br>
<br>

## **3 Core Web Vitals**
### **LCP (Largest Contentful Paint)**
It refers to the **loading performance of a site**. Largest Contentful Paint does not measure the time it takes for a site to fully load, but rather the **largest content element on the page**. This can be an image, video, animation, game, etc. <br>
A ‘Good’ LCP score is 2.5 seconds or less <br>
*Earlier the First Meaningful Content metric was used, but is now less used since the first is not likely to be the most important content.*<br>

**Ways to improve LCP:**
- Slow loading resources on the page that should be optimised by **compressing** text, resizing images etc
- Render-blocking JavaScript and CSS can slow down load times, so you want to reduce blocking time by **minifying** your files, **deferring** non-critical files and inline critical CSS.
- Slow server response times means that a browser waits longer to receive content from the server. You can use TTFB (Time to First Byte) to measure response times and improve your TTFB by optimising the server, routing to a CDN, using signed exchanges and caching assets

<br>

### **FID (First Input Delay)**
**The time it takes for a browser to acknowledge the user’s first interaction on the page**. A quicker response time leads to a better user experience that feels competent and satisfying. The First Input Delay measures mouse clicks, trackpad taps, and key presses that require the browser to respond. 
A FID score of anything under 100 ms scores ‘Good’.

**Ways to improve FID:**
- Removing unused JavaScript and CSS, minifying and compressing files
- Eliminating render-blocking resources
- Break up long tasks into smaller, asynchronous tasks
- Generate as much content as you can statically, server-side
- Explore the on-demand loading of third-party code

<br>

### **CLS (Cumulative Layout Shift)**
Observes **how many visible elements on a webpage shift around as the page loads, and by how much**.<br>
A **layout shift** occurs any time a visible element changes its position from one rendered frame to the next.<br>
If a new element is added to the DOM or an existing element changes size, it doesn't count as a layout shift—as long as the change doesn't cause other visible elements to change their start position. Elements with changed positions are called *unstable elements*.
<br>

Layout shifts that occur within 500 milliseconds of user input will have the `hadRecentInput` flag set, so they can be excluded from calculations. The `hadRecentInput` flag will only be true for *discrete* input events like `tap`, `click`, or `keypress`. *Continuous interactions* such as `scrolls`, `drags`, or `pinch` and `zoom gestures` are not considered "recent input".<br>
The CSS `transform` property allows you to animate elements without triggering layout shift.<br>
Anything below 0.1 is a ‘Good’ score.

**Ways to improve CLS:**
- Including size attributes for images and videos
- Using CSS aspect ratio boxes for the given space
- Avoid overlapping content over existing content
- Avoid changing `height`, `width`, `top`, `right`, `bottom`, `left` properties, instead use `transform: scale()` and `transform: translate()`

<br>
<br>

## **Microtasks and Macrotasks**
A **task *(or macrotask)*** is any JavaScript code scheduled to be run by the standard mechanisms such as initially starting to execute a program or an event triggering a callback. Other than by using events, you can enqueue a task by using `setTimeout()` or `setInterval()`.

<br>

When executing tasks from the **task queue**, the runtime executes each task that is in the queue at the moment a new iteration of the event loop begins. Tasks added to the queue after the iteration begins will not run until the next iteration.<br>
**Each time a task exits, and the execution context stack is empty, each microtask in the microtask queue is executed**, one after another. The difference is that **execution of microtasks continues until the queue is empty, even if new ones are scheduled** in the meantime. In other words, microtasks can enqueue new microtasks and those new microtasks will execute before the next task begins to run, and before the end of the current event loop iteration.
**The application environment is basically the same *(no mouse coordinate changes, no new network data, etc)* between microtasks**.
<br>
<br>

**Examples:**
- macrotasks: `setTimeout`, `setInterval`, `setImmediate`, `requestAnimationFrame`, `I/O`, `UI rendering`
- microtasks: `process.nextTick`, `Promise`, `queueMicrotask`, `MutationObserver`

<br>
<br>

## **How does the browser run JavaScript?**
Every browser has developed its own JS engine.

- Chrome - **V8**
- Safari - JavaScriptCore
- Firefox - SpiderMonkey

When V8 compiles JavaScript code:
- 1: The V8 parser **generates an abstract syntax tree** (AST). A syntax tree is a representation of the syntactic structure of the JavaScript code.
- 2: **Ignition**, the interpreter, **generates bytecode** from this syntax tree. (https://astexplorer.net/)
- 3: **TurboFan**, the optimizing compiler, eventually takes the bytecode and **generates optimized machine code from it**.

V8 can **detect functions that are used more frequently than others** and compile them using type information from previous executions.<br>
V8 **reuses object shapes to avoid duplicating dictionaries**. It separates an object's structure from the values itself with Object Shapes (or Maps internally) and a vector of values in memory. <br>
**When a type change is detected** during TurboFan's bytecode execution for optimization, **V8 performs deoptimization**. Which means that it throws away previously optimized code, **goes back to the interpreted code, updates type feedback, and resumes execution**. 

<br>

**High level JS code:**
```
function incrementX(obj) {
  return 1 + obj.x;
}
incrementX({x: 42});
```
V8's `parser` converts the above JS code into an: <br>
**Abstract Syntax Tree**
```
{
  "type": "Program",
  "start": 0,
  "end": 70,
  "body": [
    {
      "type": "FunctionDeclaration",
      "start": 0,
      "end": 48,
      "id": {
        "type": "Identifier",
        "start": 9,
        "end": 19,
        "name": "incrementX"
      },
      "expression": false,
      "generator": false,
      "async": false,
      "params": [
        {
          "type": "Identifier",
          "start": 20,
          "end": 23,
          "name": "obj"
        }
      ],
      "body": {
        "type": "BlockStatement",
        "start": 25,
        "end": 48,
        "body": [
          {
            "type": "ReturnStatement",
            "start": 29,
            "end": 46,
            "argument": {
              "type": "BinaryExpression",
              "start": 36,
              "end": 45,
              "left": {
                "type": "Literal",
                "start": 36,
                "end": 37,
                "value": 1,
                "raw": "1"
              },
              "operator": "+",
              "right": {
                "type": "MemberExpression",
                "start": 40,
                "end": 45,
                "object": {
                  "type": "Identifier",
                  "start": 40,
                  "end": 43,
                  "name": "obj"
                },
                "property": {
                  "type": "Identifier",
                  "start": 44,
                  "end": 45,
                  "name": "x"
                },
                "computed": false,
                "optional": false
              }
            }
          }
        ]
      }
    },
    {
      "type": "ExpressionStatement",
      "start": 49,
      "end": 69,
      "expression": {
        "type": "CallExpression",
        "start": 49,
        "end": 68,
        "callee": {
          "type": "Identifier",
          "start": 49,
          "end": 59,
          "name": "incrementX"
        },
        "arguments": [
          {
            "type": "ObjectExpression",
            "start": 60,
            "end": 67,
            "properties": [
              {
                "type": "Property",
                "start": 61,
                "end": 66,
                "method": false,
                "shorthand": false,
                "computed": false,
                "key": {
                  "type": "Identifier",
                  "start": 61,
                  "end": 62,
                  "name": "x"
                },
                "value": {
                  "type": "Literal",
                  "start": 64,
                  "end": 66,
                  "value": 42,
                  "raw": "42"
                },
                "kind": "init"
              }
            ]
          }
        ],
        "optional": false
      }
    }
  ],
  "sourceType": "module"
}
```
Ignition takes this and converts it *(by executing a preorder traversal [root, left, right])* into: <br>
**Bytecode**
```
30 S> 0000034436412196 @    0 : 0d 01             LdaSmi [1]
      0000034436412198 @    2 : c3                Star0
45 E> 0000034436412199 @    3 : 2d 03 00 01       LdaNamedProperty a0, [0], [1]
39 E> 000003443641219D @    7 : 38 fa 00          Add r0, [0]
47 S> 00000344364121A0 @   10 : a8                Return
```
Which then TurboFan compiles into **machine code**.

<br>
<br>

## **Promise.all(), Promise.allSettled, Promise.race()**
<br>

### **Promise.all():**
It's a method that takes an **iterable of promises as an input, and returns a single Promise** that resolves to an array of the results of the input promises.<br>
**If one of the promises is rejected, then the entire process is halted and the failure callback is called.**
<br>
`Promise.all()` will run all your promises until one of the following conditions are met:
- All of them resolve, which would, in turn, resolve the promise returned by the method
- One of them fails, which would immediately reject the promise returned

<br>

### **Promise.allSettled():**
This method **will not fail once the first promise is rejected**. Instead, it’ll return a **list of** values. These values will be **objects, with two properties**:
- The **status** of the returned promise (either `"rejected"` or `"fulfilled"`)
- The **value** of the fulfilled promise **or** the **reason** why a promise was rejected
<br>
*Note: A promise is settled once it gets either resolved or rejected — otherwise, it’s pending.*
<br>

### **Promise.race():**
This method **returns a promise that fulfills or rejects as soon as one of the promises in an iterable array is fulfilled or rejected**.
When any one of the promises passed into the method is **settled** (i.e., either fulfilled or rejected, but not pending), the method **returns a promise that fulfills or rejects with the value or reason from that promise**.
<br>

**Use:** You may want to have several data sources so that you can try to query them all in search of **getting data from the fastest one**, depending on network traffic or other external factors.

<br>
<br>

## **stopPropagation/stopImmediatePropagation/preventDefault**
- `event.stopPropagation` will prevent any parent handlers from being executed
- `event.stopImmediatePropagation` will prevent any parent handlers and also any other handlers from executing
- `event.preventDefault` This method is used to stop the browser’s default behavior when performing an action of an HTML element (https://jsfiddle.net/rayaqin/3h7anx05/)

<br>
<br>

## **.addEventListener()**
The `addEventListener(type, listener, options)` method of the EventTarget interface **sets up a function that will be called whenever the specified event is delivered to the target** (which could be any event target).
<br>

Events are fired to notify code of *"interesting changes"* that may affect code execution. These can arise from user interactions such as using a mouse or resizing a window, changes in the state of the underlying environment (e.g. low battery or media events from the operating system), and other causes.
Event `type`s can be found in this article: https://developer.mozilla.org/en-US/docs/Web/Events
<br>

The event listener can be specified as either a callback function or an object whose `handleEvent()` method serves as the callback function.

<br>
<br>

## **useSelector hook**
It is one of **React Redux**'s own **custom hooks**. It allows you to **extract data from the Redux store state**, using a selector function.<br>
The selector is **similar to the mapStateToProps** argument to connect **conceptually**.<br>
The selector **will be called with the entire Redux store state as its only argument**,  and will **run whenever the function component renders and** is also subscribed to the Redux store, so it runs **whenever an action is dispatched**.<br>
*The selector function **should be pure** since it is potentially executed multiple times and at arbitrary points in time.*
<br>

```
import React from 'react'
import { useSelector } from 'react-redux'

export const CounterComponent = () => {
  const counter = useSelector((state) => state.counter)
  return <div>{counter}</div>
}
```

- The selector **may return any value** as a result, not just an object. The return value of the selector will be used as the return value of the `useSelector()` hook.
- When an action is dispatched, `useSelector()` will do a **strict (===) reference comparison of the previous selector result value and the current result value** *(`connect` uses shallow comparison)*. **If they are different, the component will be forced to re-render**. If they are the same, the component will not re-render.
- The selector function **does not receive an ownProps argument**. However, props can be used through closure or by using a curried selector.
- Extra care must be taken when memoizing selectors.

You may call `useSelector()` multiple times within a single function component. **Each call to `useSelector()` creates an individual subscription to the Redux store**. Because of the React update batching behavior used in React Redux v7, a dispatched action that causes multiple `useSelector()`s in the same component to return new values should only result in a single re-render.

<br>

With mapState, all individual fields were returned in a combined object. It didn't matter if the return object was a new reference or not, **`connect()` just compared the individual fields. With `useSelector()`, returning a new object every time will always force a re-render by default**.

<br>

**The connect API is not recommended anymore** <br>
The existing `connect` API still works and will continue to be supported, but the hooks API is simpler and works better with TypeScript.

<br>
<br>

## **useDispatch hook**
It is one of **React Redux**'s own **custom hooks**. **It returns a reference to the dispatch function from the Redux store. You may use it to dispatch actions as needed**.

```
import React from "react";
import { render } from "react-dom";
import { createStore } from "redux";
import { Provider, useSelector, useDispatch } from "react-redux";

const userReducer = (state, action) => {
  switch (action.type) {
    case "addUserByName":
      return { users: [...state.users, { name: action.payload }] };
    case "removeUserByName":
      return { users: state.users.filter((u) => u.name !== action.payload) };
    default:
      return state;
  }
};

const store = createStore(userReducer, {
  users: [
    { name: "Robb" },
    { name: "Arya" },
    { name: "Rickon" },
    { name: "Bran" }
  ]
});

const FunctionalReduxCapableComponent = () => {
  const users = useSelector((state) => state.users);
  const dispatch = useDispatch();

  return (
    <div>
      {users.map((u) => (
        <div>{u.name}</div>
      ))}
      <hr />
      <button
        onClick={() => dispatch({ type: "addUserByName", payload: "Jon" })}
      >
        Add Jon
      </button>
    </div>
  );
};

render(
  <Provider store={store}>
    <FunctionalReduxCapableComponent />
  </Provider>,
  document.getElementById("root")
);
```

*Note1: When passing a callback using dispatch to a child component, you may sometimes want to memoize it with `useCallback`. If the child component is trying to optimize render behavior using `React.memo()` or similar. This **avoids unnecessary rendering** of child components due to the changed callback reference.*

*Note2: The dispatch function reference will be stable as long as the same store instance is being passed to the `<Provider>`. Normally, that store instance never changes in an application. To avoid linter errors, it is safe to add dispatch to dependency arrays.*

<br>
<br>

## **Render batching in react**
React automatically batches state updates that occur in React event handlers. Since React event handlers make up a very large portion of the code in a typical React app, this means that **most of the state updates** in a given app **are actually batched for optimization**. For example state updates in `useEffect` callbacks are **queued up**, and flushed at the end of the "Passive Effects" phase once all the `useEffect` callbacks have completed.

<br>
<br>

## **Shadow DOM**
An important aspect of web components is encapsulation — being able to keep the markup structure, style, and behavior hidden and separate from other code on the page so that different parts do not clash, and the code can be kept nice and clean. The Shadow DOM API is a key part of this, **providing a way to attach a hidden separated DOM tree to an element** of the regular DOM.<br>
The browser already hosts its own internal shadow DOM for some elements *(textarea,input)*.<br>
There is a list of elements that cannot host a shadow tree (where it doesn’t make sense, or where one is already defined).<br>
**None of the code inside a shadow DOM can affect anything outside it, allowing for handy encapsulation.**

- **Shadow host**: The regular DOM node that the shadow DOM is attached to.
- **Shadow tree**: The DOM tree inside the shadow DOM.
- **Shadow boundary**: the place where the shadow DOM ends, and the regular DOM begins.
- **Shadow root**: The root node of a shadow tree.

You can **attach a shadow root to any element** using the `Element.attachShadow()` method.
```
let shadow = elementRef.attachShadow({mode: 'open'});
let shadow = elementRef.attachShadow({mode: 'closed'});
```
Where `open` means that you **can access the shadow DOM using JavaScript** written in the main page context, and with `closed` set, you **won't be able to access the shadow DOM from the outside**, so `myCustomElem.shadowRoot` returns null.

<br>
<br>

## **Real life use for closures**
JS achieves modularity by using closures, since **a closure is a function with access to it’s parent function’s scope, that has access even after the parent finished it’s execution**.<br>
React hooks make use of closures, and demonstrate the effective use of closures. For example `useState` exposes a `setState` function for outside use, and allows manipulation of the state variable, meaning we can access and manipulate it’s inner scope.

<br>
<br>

## **JavaScript and programming paradigms**
OOP - prototypal inheritance
Functional programming - closures, first class functions, lambdas

<br>
<br>

## **What is functional programming?**
It is a programming paradigm that promotes **pure functions**, avoiding side-effects, simple and **single-purpose function compositions** (Lisp is a functional programming language).
In JavaScript: first-class functions.

<br>
<br>

## **What is a side-effect in programming**
A side effect is the modification of state through the invocation of a function or expression. In order for a function or expression to have a side effect, the state it modifies should be out of its local scope. https://xkcd.com/1790/


<br>
<br>

## **How to call async functions that only need to be run once in useEffect**
```
useEffect(() => {
  (async () => await getCharacters())().then((response: Character[]) =>
    setCharacters(response)
  );
}, []);
```

<br>
<br>

## **React.lazy and React.Suspense**
The `React.lazy` function lets you render a **dynamic import** as a regular component. <br>
This will automatically **load** the bundle containing **the dynamically imported component when it is first rendered**.<br>
Any lazy-loaded component needs to be wrapped in React's `Suspense` component.
This wrapper renders a specified fallback component that will be shown while the component is loading.

```
const MyComponent = React.lazy(() => import('./MyComponent'));

const App = () => {
  return (
    <Suspense fallback={<span>Loading...</span>}>
      <MyComponent />
    </Suspense>
  );
};
```
*Note: You can't use named exports with `React.lazy`*

<br>
<br>

## **Types vs Interfaces in TypeScript**
- Primitives and Unions can only be used with types and not with interfaces: <br>
`type T = string`<br>
`type T2 = string | number`<br>
`type T3 = T | T2`<br>

- TypeScript compiles and **merges** two or more **interfaces with the same name** into just one declaration *(while types would throw an error)*
```
interface App {
  name: string
}
interface App {
  type: 'mobile' | 'web' | 'desktop'
}
// =>
const app: App = {
  name: 'facebook',
  type: 'mobile'
}
```
- Intersection combines multiple types into one type via the `&` operator, but not in case of interfaces
```
interface Developer {
  devName: string
}
interface Tester {
  testerName: string
}
type Employee = Developer & Tester
```
- Inheritance can be achieved by using interfaces, but not with types

<br>
<br>


## **Generics in TypeScript**
To create a component that can work over a variety of types rather than a single one.<br>
While using any is certainly generic in that it will cause the function to accept any and all types for the type of arg, we actually are losing the information about what that type was when the function returns.<br>
A type argument can be explicitly provided to a function with generic type parameter *(which might be necessary in case of more complex types)*:
```
function identity<Type>(arg: Type): Type {
  return arg;
}
let output = identity<string>("myString");
```
Or we could let TypeScript infer the type based on the value's type automatically:
```
let output = identity("myString");
```

<br>
<br>

## **Page optimization methods**
- Proper choice of algorithm for each time-consuming task
- Time to first byte decrease by lazy loading
- Reduce the size of your larger css, html and js files by compression *(not images)*
- Minify code by removing spaces, commas, comments *(CSSNano, UglifyJS)*
- Reduce the number of redirects if possible
- Don't use render-blocking JS (make JS async, defer loading of js, inline external js)
- Leverage browser caching (recommended a minimum cache time of one week and preferably up to one year for static assets, or assets that change infrequently)
- server-side-rendering (when less dynamic data, read preference is more than write, simple UI with few pages, concerned about user's net speed)
- use of CDNs
- CSS sprites and **optimized images** that are not larger or more detailed than they need to be
- avoid unnecessary page re-renders
- virtual scrolling

<br>
<br>

## **Virtual scrolling**
Non-visible items in a long list are emulated (virtualized) via top and bottom padding elements, which are empty but have some height necessary to provide consistent scrollbar parameters.
Each time the user scrolls out of the set of visible items, the content is rebuilt: new items are fetched and rendered, old ones are destroyed, padding elements are recalculated.

<br>
<br>

## **useLayoutEffect()**
The signature is identical to `useEffect`, but **it fires synchronously after all DOM mutations** *(as opposed to `useEffect`, which runs async after screen paint)*. Use this to **read layout from the DOM and synchronously re-render**. Updates scheduled inside `useLayoutEffect` will be flushed synchronously, before the browser has a chance to paint, thus **avoiding** possibly **flickering UI**.<br>
Prefer the standard `useEffect` when possible to avoid blocking visual updates.<br>
Typically one shouldn't put `useLayoutEffect` in a server-rendered component.

<br>
<br>

## **useDeferredValue()**
It accepts a value and **returns a new copy of the value that will defer to more urgent updates**. If the current render is the result of an urgent update, like user input, **React will return the previous value and then render the new value after the urgent render has completed**.
The benefits to using `useDeferredValue` is that React will work on the **update as soon as other work finishes**.
<br>

As opposed to `useTransition()`, with `useDeferredValue()`, **you don't wrap the state updating code but instead the value that's in the end generated or changed because of the state update**.<br>
It makes sense to **prefer `useTransition()` if** you have some state update that should be treated with a lower priority and **you have access to the state updating code**. If you don't have that access, use `useDeferredValue()`.

```
function ProductList({ products }) {
  const deferredProducts = useDeferredValue(products);
  return (
    <ul>
      {deferredProducts.map((product) => (
        <li>{product}</li>
      ))}
    </ul>
  );
}
```

<br>
<br>

## **useTransition()**
It can be used to **tell React that certain state updates have a lower priority** *(i.e., all other state updates or UI rendering triggers have a higher priority)*.
<br>

When calling `useTransition()`, you get back an array with exactly two elements: An **isPending** boolean value, telling you whether the low-priority state update is still pending *(this can be used to **show some fallback content** while waiting for the main state update)*, and a **`startTransition()` function** that can be **wrapped around a state update** to tell React, that it is a low-priority update.

```
function App() {
  const [isPending, startTransition] = useTransition();
  const [filterTerm, setFilterTerm] = useState('');

  const filteredProducts = filterProducts(filterTerm);

  function updateFilterHandler(event) {
    startTransition(() => {
      setFilterTerm(event.target.value);
    });
  }

  return (
    <div id="app">
      <input type="text" onChange={updateFilterHandler} />
      {isPending && <p>Updating List...</p>}
      <ProductList products={filteredProducts} />
    </div>
  );
}
```
The `setFilterTerm()` state updating function is **wrapped by `startTransition()`** and therefore React treats this state updating code with a **lower priority**. In the demo, this means that the **input field stays responsive** and react instantly to keystrokes. Without the use of `useTransition()` the app can get unresponsive, especially on slower devices.
<br>

Only use it **when you have a slow user interface**, especially **on older devices**, or in a situation where you have no other solution to use. This is because it takes up extra performance.

<br>
<br>