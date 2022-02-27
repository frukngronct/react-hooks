# react-hooks

## Setting up a new project (without create-react-app)

1. Execute `npm init` in the root directory. Provide the paramater -y to skip all the questions and use the default answers.
2. Add and configure prettier extension to enable auto-formatting. Steps: (1) run `npm install -D prettier` (2) create `.prettierrc` file wih the default configuration in the root directory. (3) create script to ease executing prettier (refer to commit)
3. Install prettier VSCode extension (Prettier - Code formatter by esbenp.prettier-vscode). Enable the configurations (command comma) (1) turn on Editor: Format On Save (2) turn on Prettier: Require Config
4. Add and configure eslint extension for enforce code styles (1) run `npm install -D eslint eslint-config-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-react-hooks` (2) create `.eslintrc.json` file with the default configuration in the root directory (refer to commit). (3) create a script to run eslint from npm ``
5. Install ESLint extension for VSCode (ESLint by Dirk Baeumer).
6. Install babel `npm install -D @babel/core @babel/preset-react @babel/preset-env @babel/eslint-parser` and add default config in the `.babelrc` file (refer to commit)
7. Add extra dependencies (like react and react-dom)
8. Add a bundler of preference (for this project parcel will be used) and create the respective script (refer to commit).

## useState

useState in its most basic usage is a container that permits you to keep state in components. It takes one parameter, that could be of any value, and returns a tuple. The first element is the value itself and the second a function to mutate that value.

Every time the function to mutate the value is called a rerender cycle is kicked off - even if the same value is set again and nothing changes. In case nothing changes react might decide to no complete the rerender cycle.

## useEffect

Initially useEffect is used to schedule something to happen after the first render. It is also used to determine what should happen when a specific state changes.

You never know exactly when it is going to run (even though in theory just some milliseconds after the render).

It is also import to clean up after yourself to not risk memory leaks and unexpected errors. To do so, return a function at the end of the useEffect callback function. We don't need to clean up everything though. Typical things that need clean ups: setTimeout, rx js and websockets - things that you subscribe.

## useContext

Prop drilling problem: pass properties down from one component to another until it reaches the ones that really needs it - and due to react's componentization way there could be many levels.

One of the great things about react is that it is explicit thus easy to follow and trace back. useContext is great because it works around the pop drilling problem, however it introduces a level of indirection hindering readability, explicitness and maintainability. It has to be used with care and overuse avoided.

## useRef

It is meant to save pieces of state throughout various rendering cycles. It is a container that does not get modified by the react lifecycle methods.

It is a container and can store anything.

## useReducer

Disclaimer: it is not redux - but similar indeed from functionality perspective :P. It is basically a thin layer on top of useState that allows you to treat your state like Redux store.

At first glance, reducers (or redux) might seem more complicated than using useEffect. However it helps to isolate in a single place one thing that usually is a big source of bugs - data mutation (or state changes). It enables you to create unit tests that ensure that the state change is working as expected isolatedly from anything else. Moreover it centralises and makes it easy to understand how the state is changing.

In other words, you are able to detach / decouple state change from the display layer.

useReducer is usually used for local pieces of state and redux for global state.

## useMemo

When (re)rendering components, there might be some computations that are very expensive and we don't want them to be recomputed if the value(s) that they rely on have not changed. `useMemo` is used for this exact purpose.

useMemo hook takes two parameters: the first the function with the expensive processing, the second the variables that should trigger its recomputation (similar fashion to useEffect).

The main difference between useMemo and useEffect is that useEffect happens asynchronously and useMemo synchronously - you schedule the execution upon renderisation. Which one to use depends on the use case.

## useCallback

It is at some extend similar to useMemo, however it is used when you have to pass down an expensive function to a child component but you don't want the child component to re-render (and call the function) every time the parent re-renders. You only want it to do so when in fact something has changed and you need the component to re-render.

Due to the nature of hook components, it relies heavily on inner functions, therefore new functions are created every single time there is a re-render cycle (independently of local data due access to closure anatomy). It causes the child component to receive a different function every single time (even if nothing has changed) causing the expensive re-render - despite the needed `memo` wrapping (after all it thinks that the function changed).

`useCallback` kinda holds to the original function definition and gives the same function back every single time. It only updates the function in the variable(s) it relies on updates (it takes a second parameters in a similar fashion that `useMemo` and `useEffect` do).

_be careful you rely on local data provided by closure to specify exactly when react should update the function reference_

If the function wasn't an inner function `useCallback` wouldn't be necessary.
