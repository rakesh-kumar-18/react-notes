# Redux:

- **Redux** is a pattern and library for managing and updating application state, using events called "actions". It serves as a centralized store for state that needs to be used across your entire application, with rules ensuring that the state can only be updated in a predictable fashion.
- Redux helps you manage "global" state - state that is needed across many parts of your application.

## One way data flow:

### State Management:

- State: State describes the condition of the app at specifc point in time.
- View: The UI rendered based on the state.
- Action: When some event occurs, the state is updated based on that event.
- The UI re-renders based on the new state.

    <img src="https://redux.js.org/assets/images/one-way-data-flow-04fe46332c1ccb3497ecb04b94e55b97.png" alt="data-flow" align="center" width="500px">

### Terminology:

1. Actions:
    - An action is a plain JavaScript object that has a ``type`` field. You can think of an action as an event.
    - The ``type`` field should be a string that gives this action a descriptive name, like "``todos/todoAdded``". We usually write that type string like "``domain/eventName``", where the first part is the feature or category that this action belongs to, and the second part is the specific thing that happened.
    - An action object can have other fields with additional information about what happened. By convention, we put that information in a field called ``payload``.
    - A typical action object look like this:
  
        ```javascript
        const addTodoAction = {
            type: "todos/todoAdded",
            payload: "Learn Radux"
        }
        ```

2. Action Creators:
    - An **action creator** is a function that creates and returns an action object. We use this so we don't have to write the action object manually everytime.
   
       ```javascript
        const addTodo = (todoText) => {
            return {
                type: "todos/todoAdded",
                payload: todoText
            }
        }
        ```

3. Reducers:
    - A **reducer** is a function that receives the current ``state`` and an ``action`` object, decides how to update the state if necessary, and returns the new state: ``(state, action) => newState``. You can think of a reducer as an event listener which handles events based on the received action (event) type.
    - The logic inside reducer functions typically follows the same series of steps:
        - Check to see if the reducer cares about this action.
            - If so, make a copy of the state, update the copy with new values, and return it.
        - Otherwise, return the existing state unchanged.
    - Here's a small example of a reducer, showing the steps that each reducer should follow:
    
        ```javascript
        const initialState = { value: 0 }

        function counterReducer(state = initialState, action) {
            switch(action.type) {
                case "counter/increrment": {
                    return {
                        ...state,
                        value: state.value + 1
                    }
                }
                default: {
                    return state
                }
            }
        }
        ```

4. Store:
    - The current Redux application state lives in an object called the **store**.
    - The store is created by passing in a reducer, and has a method called ``getState`` that returns the current state value:

        ```javascript
        import { configureStore } from '@reduxjs/toolkit'

        const store = configureStore({ reducer: counterReducer })

        console.log(store.getState())
        // {value: 0}
        ```

5. Dispatch:
    - The Redux store has a method called ``dispatch``. The only way to update the state is to call ``store.dispatch()`` and pass in an action object. The store will run its reducer function and save the new state value inside, and we can call ``getState()`` to retrieve the updated value:

        ```javascript
        store.dispatch({ type: 'counter/increment' })

        console.log(store.getState())
        // {value: 1}
        ```

        >You can think of dispatching actions as "triggering an event" in the application. Something happened, and we want the store to know about it. Reducers act like event listeners, and when they hear an action they are interested in, they update the state in response.
    - We typically call action creators to dispatch the right action:

        ```javascript
        const increment = () => {
        return {
            type: 'counter/increment'
        }
        }

        store.dispatch(increment())

        console.log(store.getState())
        // {value: 2}
        ```

## Redux Application Data Flow:

- Initial setup:
    - A Redux store is created using a root reducer function.
    - The store calls the root reducer once, and saves the return value as its initial ``state``.
    - When the UI is first rendered, UI components access the current state of the Redux store, and use that data to decide what to render. They also subscribe to any future store updates so they can know if the state has changed.
- Updates:
    - Something happens in the app, such as a user clicking a button.
    - The app code dispatches an action to the Redux store, like ``dispatch({type: 'counter/increment'})``.
    - The store runs the reducer function again with the previous ``state`` and the current ``action``, and saves the return value as the new ``state``.
    - The store notifies all parts of the UI that are subscribed that the store has been updated.
    - Each UI component that needs data from the store checks to see if the parts of the state they need have changed.
    - Each component that sees its data has changed forces a re-render with the new data, so it can update what's shown on the screen.

- Here's what that data flow looks like visually:

    <img src="https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif" alt="redux-data-flow" align="center" width="500px">

# Redux Toolkit:

- **Redux toolkit** is a package which is intended to be the standard way to write Redux logic. It is an abstraction layer that wraps around the redux core.

## Process to setup and use Redux Toolkit with React:

1. Create a Redux store with ``configureStore``:
    - ``configureStore`` accepts ``reducer`` function as a named argument.
    - ``configureStore`` automatically sets up store with good default settings.
2. Provide the Redux store to the React application components:
    - Put a React-Redux ``<Provider>`` component around your ``<App />``.
    - Pass the Redux store as ``<Provider store={store}>``.
3. Create a Redux "**slice**" reducer with ``createSlice``:
    - Call ``createSlice`` with a string name, an initial state, and named reducer functions.
    - Reducer functions may "**mutate**" the state using Immer.
    - Export the generated slice reducer and action creators.
4. Use the React-Redux ``useSelector/useDispatch`` hooks in React components:
    - Read data from the store with the ``useSelector`` hook.
    - Get the ``dispatch`` function with the ``useDispatch`` hook, and dispatch actions as needed.
