# Mini redux

```js
function createStore(reducer, initialState) {
    var currentReducer = reducer;
    var currentState = initialState;
    var listener = () => {};
 
    return {
        getState() {
            return currentState;
        },
        dispatch(action) {
            currentState = currentReducer(currentState, action);
            listener();
            return action;
        },
        subscribe(newListener) {
            listener = newListener;
        }
    };
}
```


[как работает combineReducer](https://www.tune-it.ru/web/bleizard/blog/-/blogs/ponimaem-kak-rabotaet-funkcia-combinereducer-iz-redux)