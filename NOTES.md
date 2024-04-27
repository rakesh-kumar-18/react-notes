# React:

**React** is a front-end JavaScript library for building web and native user interfaces based on components.

## Components:

- React apps are made out of components. A component is a piece of the UI (user interface) that has its own logic and appearance. A component can be as small as a button, or as large as an entire page.
- React components are JS functions that return markup.

    ```jsx
    function Component() {
        return (
            <h1>React</h1>
        );
    }
    ```

## JSX:

- JSX stands for **JavaScript XML**. JSX allows us to write HTML in React. JSX makes it easier to write and add HTML in React.
- JSX is stricter than HTML. You have to close tags like ```<br />```. Your component also can't return multiple JSX tags. You have to wrap them into a shared parent, like a ```<div>...</div>``` or an empty ```<>...</>``` wrapper.

## Display Data:

- JSX lets you write HTML in JavaScript. Curly braces let you escape back to JavaScript so that you can embed some variable from your code and display it to the user.
- You can also **escape into JavaScript** from JSX attributes using same curly braces syntax.

    ```jsx
    function Component() {
        const user = {
            name: "xyz",
            email: "xyz@github.com",
            imgUrl: "https://profile.png"
        }

        return (
            <h1>{user.email}</h1>
            <img className="profile" src={user.imgUrl} />
        )
    }
    ```

## Conditional rendering:

- There is three type of syntax to write condition in React. It is no different from regular JavaScript code.

    1. if statement
    2. conditional operator ( **?** ):

        ```jsx
        <div>
            {isLoggedIn ? (
                <AdminPanel />
            ) : (
                <LoginForm />
            )}
        </div>
        ```
    3. logical and syntax ( **&&** ):

        ```jsx
        <div>
            {isLoggedIn && <AdminPanel />}
        </div>
        ```

## Event handling:

- You can respond to events by declaring event handler functions inside your components:

    ```jsx
    function Component() {
        const handleClick = () => {
            console.log("button got clicked")
        }
        
        return (
            <>
                <button onClick={handleClick}>Click Me!</button>
            </>
        )
    }
    ```

## State:

- State is act as a **component's memory**. sometimes components need to change the UI as a result of an interaction. To do this, add state to your component.
- Components need to remember things. In React, this kind of component-specific memory is called **state**.
- Remember, State is **private to the component**. If you render same component multiple times. each copy will have completely isolated state! Changing one of them will not affect the other.

    ```jsx
    const [state, setState] = useState(initialState)
    ```

- When you call '**useState**' hook, you are telling React that you want this component to remember something.

## Why regular local variable can't be used as state:

- Local variable don't persist between renders.
- Changes to local variable won't trigger re-render.

- To update a component with new data, two things need to happen:
    1. Retain the data between renders.
    2. Trigger React to render the component with new data (re-rendering).

## Render and Commit:

1. Triggering a render:
    - There are two reasons for a component to render:
        - Initial Render.
        - State Updated of any specific component or its ancestors.

2. Rendering the component:
    - After you trigger a render, React calls your components to figure out what to display on screen. "**Rendering**" is React calling your components.
        - On initial render, React will call the root component.
        - For subsequent renders, React will call the function component whose state update triggered the render. It will also render all the nested components.
    
3. Commiting to the DOM.
    - After rendering (calling) your components, React will modify the DOM.
        - For the initial render, React will use the **appendChild()** DOM API to put all the DOM nodes it has created on screen.
        - For re-renders, React will apply the minimal necessary operations (calculated while rendering!) to make the DOM match the latest rendering output.

- After rendering is done and React updated the DOM, the browser will repaint the screen. This process is known as **browser rendering**.

## Hooks:

- Hooks are special functions that start with '**use**'. They let you '**hook into**' React features like state.
- You can only call Hooks at the top of your components (or other Hooks).

## Props:

- React components uses **props** to communicate with each other. A parent component can pass some information/data to its child component through props.
- Props are information that you pass to a JSX tag. For example, ``className``, ``src``, ``alt`` are some props you can pass to an ``<img>`` tag.
- The props you can pass to a html tag are predefined. But, you can also pass any props to your own components to customize them.

    ```jsx
    // Parent.jsx
    function Parent() {
        return (
            <Child
                name="React"
                version={18}
            />
        )
    }
    ```

    ```jsx
    // Child.jsx
    function Child({name, version}) {
        return (
            <>
                <h1>{name}</h1>
                <p>{version}</p>
            </>
        )
    }
    ```

- Props are the **only argument to your component!** React component functions accept a single argument, a props object.

    ```jsx
    function Child(props) {
        let name = props.name;
        let version = props.version;
        // ...
    }
    ```

- Props reflect a component's data at any point in time. However, props are '**immutable**'. you can't change props.

## Sharing data between components:

- If we want to share data from a parent component to a child component then we can do this using props. But when it comes to sharing data between two sibling/same-level components the process is little different.
- Share data between same-level components involve 2 steps:
    1. Move the states from child components to its closest parent component. This is called "**lifting the state up**".
    2. Then,pass the state down from parent to each child component as **props**, together with state handler if needed.

## Controlled and uncontrolled components:

### Uncontrolled component:

- It is common to call a component with some local state "**uncontrolled**".
- Uncontrolled components are easier to use within their parents because they require less configuration. But they're less flexible when you want to coordinate them together.

### Controlled component:

- A component is "**controlled**" when the important information in it is driven by props rather than its own local state.
- Controlled components are maximally flexible, but they require the parent components to fully configure them with props.

>When writing a component, consider which information in it should be controlled (via props), and which information should be uncontrolled (via state). But you can always change your mind and refactor later.

## Preserving and Resetting State:

- As a rule of thumb, if you want to preserve the state between re-renders, the structure of your tree needs to **match up** from one render to another. If the structure is different, the state gets destroyed because React destroys state when it removes a component from the tree.

## Resetting state at the same position:

- By default, React preserves state of a component while it stays at the same position.
- But, there are two ways to reset state when switching between them:
    1. Render components in different positions.
    2. Give each component an explicit identity with **key** prop.

## Reducer:

-  As the component grows, many state updates spread across many event handlers can get overwhelming. To reduce this complexity and keep all your logic in one easy-to-access place, you can move the state logic into a single function outside your component, called a **reducer**.
- Reducers are a different way to handle state. You can migrate from useState to useReducer in three steps:
    1. Move from setting state to dispatching actions.
        - Instead of telling React "what to do" by setting state, you specify "what the user just did" by dispatching **actions** from your event handlers.

            ```jsx
            function handleAddTask(text) {
              // "action" object
              dispatch({
                type: 'added',
                id: nextId++,
                text: text,
              });
            }

            function handleChangeTask(task) {
              // "action" object
              dispatch({
                type: 'changed',
                task: task,
              });
            }

            function handleDeleteTask(taskId) {
              // "action" object
              dispatch({
                type: 'deleted',
                id: taskId,
              });
            }
            ```

            >The object you pass to "dispatch" is called an "**action**" object.

            ```jsx
            dispatch({
                type: "what-happend",
                // other fields
            })
            ```

    2. Write a reducer function.
        - A reducer function is where you will put your state logic. It takes two arguments, the current state and the action object, and it returns the next state:

            ```jsx
            function myReducer(state, action) {
                // return next state for react to set
                switch (action.type) {
                    case "added": {
                        // return updated state
                    }
                    case "change: {
                        // return updated state
                    }
                    default: {
                        // return default state
                    }
                }
            }
            ```

    3. Use the reducer from your component.
        - Finally, you need to hook up you **reducer** fucntion  to your component. Import the ``useReducer`` Hook from React:

            ```jsx
            const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
            ```

            >The ``useReducer`` Hook is similar to ``useState``,  you must pass it an initial state and it returns a stateful value and a way to set state (in this case, the dispatch function). But it's a little different.

        - The ``useReducer`` Hook takes two arguments:
            1. A reducer function
            2. An initial state

        - And it returns:
            1. A stateful value
            2. A dispatch function (to **dispatch** user actions to the reducer)

## Prop drilling:

- When you need to pass some prop deeply through the tree, or if many components need the same prop. The nearest common ancestor could be far removed from the components that need data, and lifting state up that high can lead to a situation called "**prop drilling**".

## Context:

- Context lets the parent component make some information available to any component in the tree below it no matter how deep without passing it explicitly through props.

### Passing data deeply with context:

- It can be done in three steps:
    1. Create a context.

        ```jsx
        import { createContext } from 'react';

        export const MyContext = createContext(defaultValue);
        ```
        
        >The only argument to **createContext** is the default value. you could pass any kind of value (even an object).

    2. Use that context from component that needs the data.
        - Import the ``useContext`` Hook from React and your context from context file.
        - Remove the "prop" and read the value from the context you just imported.

        ```jsx
        import { useContext } from 'react';
        import { MyContext } from './MyContext.ts';

        export default function MyComponent({ children }) {
            const context = useContext(MyContext);
        }
        ```

        >``useContext`` tells React that the "MyComponet" component wants to read the "MyContext".

    3. Provide the context from the component that specifies the data.

        ```jsx
        import { MyContext } from './MyContext.ts';

        export default function App() {
            <MyContext.Provider value={state}>
                <Section>
                    <h1>xyz</h1>
                </Section>
                // other code(children)...
            </MyContext.Provider>
        ```

# Reconciliation:

- The algorithm React uses to diff one tree with another to determine which parts need to be changed.
- Reconciliation is the algorithm behind what is popularly understood as the "**virtual DOM**".

## React fiber architecture:

- [Article](https://github.com/acdlite/react-fiber-architecture)

## Redux:

- [Redux Notes](REDUX.md)