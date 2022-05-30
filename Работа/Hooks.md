# useCallback Polyfill
```js
const prevState = {

	prevCb: null,
	
	prevDeps: null,

};

  

const useCallback = (cb, deps) => {

	if (!prevState.deps || !deps) {
	
	prevState.cb = cb;
	
	prevState.prevDeps = deps;
	
	return cb;
	
	}
	
	if (shallowEqual(deps, prevState.deps)) {
	
	return prevState.cb;
	
	}
	
	prevState.cb = cb;
	
	prevState.deps = deps;
	
	return cb;

};
```