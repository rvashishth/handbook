## Redux

Redux maintains app state in a single object

Following all are plain javascript constructs

1. Action Creator - a function that creates action
2. Action - plain java script object `{type=Type, payload:{}})` that represent what change we want in the state
3. Dispatch - Pass copy of action object to each reducer
4. Reducer - accepts the state and action and produce new state
5. State - object that contain the state, whenever react app need a state, it can fetch it directly from the state object

Redux lib gives us following functions

1. createStore - returns the store object
2. combineReducer - accepts the json object containing all reducers

Redux Store provides

- allow access to state via `store.getState()`
- allow state to be updated via `store.dispatch(action)`
- register listeners using `store.subscribe(listenerFunction)`
- handler unregistering of listeners using function returned by `store.subscribe(listenerFunction)`

```
const dept = combineReducers({
    accounting:accountReducer,
    it:itReducerFunction,
    infra:infraReducerFunction
})
```

Now the state object will look like below. Keys in state object are equal to the keys passed to `combineReducers` function

```
{
    accounting:
    it:
    infra:
}
```

> In a Reducer never modify an existing state, instead always always return a new state object

## React-Redux

Provider, connect, mapDispatchToProps, mapStateToProp

- connect function and Provider components talk to each other via context system
- the connect function takes the data from store(from provider) and pass that as prop to underlying component. using a function that map State To Prop function
- connect component also allow us to call action creator from within underlying components, using map Dispatch to prop. remember dispatch is a function of store.
- **both mapDispatchToProp and mapStateToProp can access to ownProps** this is required to add some logic within these functions

> The state object in made available to reducer is specific to that reducer only, while the state object made available to mapStateToProp is root state object. so if we have state like

Starting react-redux 7.1 we can use hooks

- hooks provide an alternative to `connect()` hcc method and allows to subscribe to dispatch without connect
- `useSelector` hook == maptStateToProp
- `useDispatch` returns `dispatch` that can be used within function component
- using hooks no need to write a separate container component to map state and dispatch to props

## Redux Ducks Pattern

## Redux Re-Ducks Pattern

## Redux Thunk

In redux middleware are the extension which act between an action is being dispatched and it is being processed by reducers. i.e. loggin, async api calls</br>
Thunk is a redux middleware. which help us call async rest api</br>

Thunk middleware allow us to create a action creator function</br>

- Thunk function can be impure, create side effect by making api calls
- Thunk function can return another function instead of returning an action object
- this function can have access to dispatch function, and can use this function to dispatch actions

FAQ

1. **How in link we wire up state with components**</br>
   Under /state/ducks we create files for each functionality which contains Action Type, Reducer, Action Creator and default state for this functionality i.e. transactions</br>
   Import this reducer in another /state/reducer.js file. Here we pass all the reducers and create rootReducer of type RootReducer using Redux.combineReducers. Then export the root reducer. </br>
   Now we import rootReducer in /state/store.js and use Redux.createStore with applymiddleware to create and export the store</b>
   Later we import this store object and pass it as prop to React-Redux Provider component.</b>
