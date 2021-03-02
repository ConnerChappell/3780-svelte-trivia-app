<script>
	import _ from 'lodash';
	import {onMount} from 'svelte';
	export let name;







</script>

<main>
	<h1>{name}</h1>
	
	<div>
		<label for="category">Please Select a Category</label>
		<select name="category" id="category">
			<option value=""></option>
		</select>
	</div>

	<div>
		<label for="difficulty">Please Select a Difficulty</label>
		<select name="difficulty" id="difficulty">
			<option value="easy">Easy</option>
			<option value="medium">Medium</option>
			<option value="hard">Easy</option>
		</select>
	</div>

	<button>Start Game</button>

	<div>
		<h3>A Question?</h3>
		<button>Answer</button>
		<button>Answer</button>
		<button>Answer</button>
		<button>Answer</button>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>