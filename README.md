
<!-- PROJECT LOGO -->
<br />
<p align="center">
  <h1 align="center">Scalable React</h1>

  <p align="center">
    Tips, tricks, rules and constraints for making a React app scalable, testable, flexible and maintainable.
    <br />
  </p>
</p>



<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About the Project](#about-the-project)
* [How to write testable code](#how-to-write-testable-code)
* [Writing testable React components](#writing-testable-react-components)
* [Writing modular code](#writing-modular-code)
* [Writing modular React components](#writing-modular-react-components)
* [Writing independent modules](#writing-independent-modules)


<!-- ABOUT THE PROJECT -->
## About The Project
This project shows presents you with tips, tricks, rules and constraints for designing scalable, testable and maintainable React apps. Here we will also see how to make resilient apps which is so flexible that even if a part of app fails to work, other parts continue to work without issue.

## How to write testable code

TDD tends to have a simplifying effect on code, not a complicating effect. If you find that your code gets harder to read or maintain when you make it more testable, or you have to bloat your code with dependency injection boilerplate, you’re doing TDD wrong.

Writing a more testable code should simplify your code. It should require fewer lines of code and more readable, flexible, maintainable constructions. Dependency injection has the opposite effect.

The process of learning effective TDD is the process of learning how to build more modular applications. The essence of all software development is the process of breaking a large problem down into smaller, independent pieces (decomposition) and composing the solutions together to form an application that solves the large problem (composition).

Using composition allows you to design applications where code can be tested easily without using techniques like mocking. Dividing parts of your applications into independent atomic units allows you to write scalable, maintainable, and testable code.

When decomposition succeeds, it’s possible to use a generic composition utility to compose the
pieces back together. Examples:

* Function composition, e.g., lodash/fp/compose
* Component composition e.g., composing higher-order components with function composition
* State store/model composition e.g., Redux combineReducers67
* Object or factory composition e.g., mixins or functional mixins
* Process composition e.g., transducers
* Promise or monadic composition e.g., asyncPipe() , Kleisli composition with composeM() ,composeK() , etc...
* etc...

**Follow below steps to decompose large applications into independent atomic units:**
* Divide your problem (application feature) into small simple steps.

* Convert steps into very small independent pure functions. Remember that these functions will act as independent atomic units of your application which can be tested easily without mocking. These functions must be pure which means they must not access anything (such as - value, functions, objects, etc.) outside their scope. They must not directly change the value of an object (instead you can clone these objects and return cloned modified objects). Each of these functions must do only one thing which makes your code readable, testable, and maintainable.

* Combine these small independent function to solve your problem. In functional programming **compose()** utility is used to solve complex problems by dividing them into small functions and composing them together to finally solve any complex problem. The process is called function composition.

* Test these independent atomic units (functions) using unit tests and test those composed functions using integration tests.

**Function composition** is the process of applying a function to the return value of another function. In other words, you create a pipeline of functions, then pass a value to the pipeline, and the value will go through each function like a stage in an assembly line, transforming the value in some way before it’s passed to the next function in the pipeline. Eventually, the last function in the pipeline returns the final value.

## Writing testable React components

Follow below rules to make your components testable and reusable -

* Design your components without class. Functional ReactJS components are easy to test.

* Do not use states in your component instead use state management libraries like [storeon](https://github.com/amit08255/storeon). Use container components to manage states and state stores.

* Design your components small and dumb. It must not contain any logic.

* Separate I/O such as network requests from your components.

* Do not test internals of your component such as states. Your tests must be independent of internal working of your components.

* Your components should be independent and should not depend on other components or modules.

* Receive all event handlers as props which makes testing easier when you want to test if your component events are working correctly.

## Writing modular code

Modules are reusable software components that form the building blocks of applications. Modularity satisfies some very important design goals, perhaps the most important of which is simplicity. When you design an application with a lot of interdependencies between different parts, it becomes more difficult to fully understand the impact of your changes across the whole system.

Another important goal of modularity is the ability to reuse your module in other applications. Well-designed modules built on similar frameworks should be easy to transplant into new applications with few (if any) changes. By defining a standard interface for application extensions and then building your functionality on top of that interface, you’ll go a long way toward building an application that is easy to extend and maintain and easy to reassemble into different forms in the future.

JavaScript modules are encapsulated, meaning that they keep implementation details private and expose a public API. That way, you can change how a module behaves under the hood without changing code that relies on it. Encapsulation also provides protection, meaning that it prevents outside code from interfering with the functionality of the module.

You can think of modules as small, independent applications that are fully functional and fully testable in their own right. Keep them as small and simple as possible to do the job that they are designed to do.

**Modules should be:**

* **Specialized:** Each module should have a very specific function. The module’s parts should be integral to solving the single problem that the module exists to solve. The public API should be simple and clean.

* **Independent:** Modules should know as little as possible about other modules. Instead of calling other modules directly, they should communicate through mediators, such as a central event-handling system or command object.

* **Decomposable:** It should be fairly simple to test and use modules in isolation from other modules. It is common to compare them to components in an entertainment system. You could have a disc player, radio receiver, TV, amplifier, and speakers, all of which can operate independently. If you remove the disc player, the rest of the components continue to function.

* **Recomposable:** It should be possible to fit various modules together in different ways to build a different version of the same software or an entirely different application.

* **Substitutable:** It should be possible to completely substitute one module with another, as long is it supplies the same interface. The rest of the application should not be adversely impacted by the change. The substitute module doesn’t have to perform the same function. For example, you might want to substitute a data module that uses REST endpoints as its data source with one that uses a local storage database.

## Writing modular React components

Follow below techniques to design modular React component:

* Write functional component wherever possible.

* No two component should know about each other.

* Components should be as small as possible.

* All components should be dumb and should not contain network requests.

* UI components should be stateless whenever possible.

* Use React [`ErrorBoundary`](https://reactjs.org/docs/error-boundaries.html) around individual component usage to control error in single component on rendering. **Always use `ErrorBoundary` around your React component, not individual JSX element.**

* When using javascript to generate user interface, wrap JavaScript code around try/catch to handle error when using SSR (server-side rendering). You can use [`ErrorBoundary`](https://reactjs.org/docs/error-boundaries.html) with try/catch to prevent both client-side rendering and server-side rendering. **The trick is simple, when using JavaScript code to generate user interface, add try/catch around the component as close as possible to contain error when using server-side rendering**.

    **Example:**
    ```js
    import React from 'react';
    import PropTypes from 'prop-types';

    const CentreCards = ({ list }) => {
        try {
            return (
                <>
                    {
                        list.map((item, index) => (
                            <div
                                key={`image-list-${index + 1}`}
                                className="col-lg-2 col-md-3 col-sm-4"
                            >
                                <img
                                    src={item.thumbnail}
                                    alt={item.label}
                                />
                            </div>
                        ))
                    }
                </>
            );
        } catch (error) {
            return <div>Error</div>;
        }
    };

    CentreCards.propTypes = {
        list: PropTypes.arrayOf(
            PropTypes.shape({
                image: PropTypes.string,
                thumbnail: PropTypes.string,
            }),
        ).isRequired,
    };

    export default CentreCards;
    ```

* Use container components to combine multiple components in order to build final user interface. Container components can be smart and use network requests and have states shared amoung all of it's child components.

* All shared states among container components should be stored in store managers like [storeon](https://github.com/amit08255/storeon).

* Never mix business logic and UI code in container components and UI components. For container components, create a file `actions.js` in same directory as container component file and keep all business logic in `actions.js` file.

## Writing independent modules

Main principal of modular programming is that all modules should be independent and not know about each other. This means that, you cannot import modules directly because doing that will make your module tightly coupled with other modules imported. To make modules independent from each other, we can use a mediator which allows communication between modules while keeping them independent. One of the simple techniques we can use is [pubsub](https://github.com/amit08255/javascript-pubsub). Pubsub messaging technique will allow communication between modules while keeping them independent.

Follow below pattern for independent communication between modules using pubsub:

* Register all modules/internal dependencies required as pubsub subscribers at app entry point. On web, you can think of every page as an entry point.

* For all required dependencies/functionalities in modules, directly publish an event which perform that using subscriber callback.

* Do not import other modules in app in a module. Only exception is that, you can import external dependencies like - React, prop-types, pubsub library. Separating a module from other modules in the app will allow you to directly reuse that module in other apps without lots of configuration, and also allows to test each module independently.

## File Structure

The following file structure is a simple structure of ReactJS NextJS app:

```
app-directory/
 ├──pages/                  * this folder contain files for every route in our app.
 │   │──index.jsx           * JSX file for index page of our app.
 │   │
 ├──components/             * this folder contain files for every UI component and styles in our app.
 │   │──button/             * Simple button component for our app.
 │      │──index.jsx        * Button UI code.
 │      │──index.style.jsx  * styled components style for button component
 │      │
 ├──containers/             * this folder contains container components.
 │   │──blogList/           * Container to display blog post list using multiple components.
 │      │──index.jsx        * container code.
 │      │──actions.js       * API requests and data processing to prepare response for UI. 
 │      │
 ├──services/               * this folder contains services (API requests, business logic etc.)
 │   │──blogs/              * Blogs API service.
 │      │──index.js         * Code to get blogs list, add blog post, edit blog post.
 │      │──utils.js         * Utility functions used in blogs services.
 │      │
 ├──stores/                 * this folder contains storeon stores code for app.
 │   │──blogs.js            * blogs store for keeping shared states of blogs page.
 │   │
 ├──utilities/              * this folder contains utility functions used by app.
 │   │──index.js            * utility functions such as - functional programming utilities like - compose.
 │   │
 │──__tests__/              * this folder contain files used by testing framework.
 │   └──index.jsx           * file which contain test for index page of our app.
 |──jest.config.js          * configuration file used by our Jest testing framework.
 │
 |──next.config.js          * NextJS app configuration file.
 │
 |──package.json            * file which contains NodeJS project dependencies and scripts.
 |
 |──server.js               * file used by NodeJS to start ExpressJS server which handles app routing, caching and compression.
 |
 └──babel.config.js         * Babel configuration file to tell babel how to handle our project file conversion to ES6.
```