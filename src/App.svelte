<script>
	import _ from 'lodash';
	import {onMount} from 'svelte';
	export let name;

	let categories = [];
	let selectedCategory = '';
	let selectedDifficulty = '';

	onMount(function() {
		fetch('https://opentdb.com/api_category.php')
			.then(res => res.json())
			.then((data) => {
				console.log(data)
				categories = data.trivia_categories
			})
	})

	async function getQuestions() {
		// todo: validate inputs
		const response = await fetch(`https://opentdb.com/api.php?amount=10&category=${selectedCategory}&difficulty=${selectedDifficulty}&type=multiple`);
		const data = await response.json();
	}

</script>


<main>
	<h1>{name}</h1>
	
	<div class="categoryContainer">
		<label for="category">Please Select a Category</label>
		<select bind:value="{selectedCategory}" name="category" id="category">
			{#each categories as category}
			<option value="{category.id}">{category.name}</option>
			{/each}
		</select>
	</div>

	<div class="difficultyContainer">
		<label for="difficulty">Please Select a Difficulty</label>
		<select bind:value="{selectedDifficulty}" name="difficulty" id="difficulty">
			<option value="easy">Easy</option>
			<option value="medium">Medium</option>
			<option value="hard">Easy</option>
		</select>
	</div>

	<button>Start Game</button>

	<div class="quizContainer">
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