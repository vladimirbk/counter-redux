<script>
	import thunk from 'redux-thunk';
	import logger from 'redux-logger';
	import { createStore, applyMiddleware, compose } from 'redux';
	import { composeWithDevTools } from 'redux-devtools-extension';
    import { changeTheme, decrement, increment, asyncIncrement } from '../scripts/redux/actions';
	import { rootReducer } from '../scripts/redux/rootReducer';

	const store = createStore(
		rootReducer, 
		composeWithDevTools(applyMiddleware(thunk, logger))
	);

	let addBtn;
	let removeBtn;
	let changeThemeBtn;
	let asyncBtn; 

	let counter = 0;

	function increase() {
		store.dispatch(increment());
	}

	function decrease() {
		store.dispatch(decrement());
	}

	function asynchronousIncrement(){
		store.dispatch(asyncIncrement());
	}

	function changeThemeColor() {
		let newTheme = document.querySelector('div').className.split(' ')[0] === 'app-container' ? 'app-container-blue' : 'app-container';
		store.dispatch(changeTheme(newTheme));
	}

	store.subscribe(() => {
		const state = store.getState();
		counter = state.counter;
		document.querySelector('div').className = state.theme.value + ' ' + document.querySelector('div').className.split(' ')[1];

		[addBtn,removeBtn,changeThemeBtn,asyncBtn].forEach((btn) => {
			btn.disabled = state.theme.disabled;
		});
	});
</script>

<main>
	<div class="app-container">
		<h1>Simple counter</h1>
		<div class="counter-container">
			<h3>Counter</h3>
		<h3 id="counter">{ counter }</h3>
		</div>
		<button id="add" bind:this={addBtn} on:click={increase}>Increase</button>		
		<button id="remove" bind:this={removeBtn} on:click={decrease}>Decrease</button>		
		<button id="async" bind:this={asyncBtn} on:click={asynchronousIncrement}>Async</button>		
		<button id="change-theme" bind:this={changeThemeBtn} on:click={changeThemeColor}>Change theme</button>		
	</div>
</main>
<style>	
	.counter-container {
		width: 100px;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}

	h1 {
		color: #535a5c;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}

	h3 {
		color: #535a5c;
		text-transform: uppercase;
		font-size: 1em;
		font-weight: 100;
	}
</style>