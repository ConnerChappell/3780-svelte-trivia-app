<script>
	import _ from 'lodash';
	import {onMount} from 'svelte';
	export let name;

	let categories = [];
	let selectedCategory = '';
	let selectedDifficulty = '';
	let questions = [];
	let displayQuestions = [];
	let correct = 0;
	let incorrect = 0;

	onMount(function() {
		fetch('https://opentdb.com/api_category.php')
			.then(response => response.json())
			.then((data) => {
				console.log(data)
				categories = data.trivia_categories
			})
	})

	async function getQuestions() {
		// todo: validate inputs
		const response = await fetch(`https://opentdb.com/api.php?amount=10&category=${selectedCategory}&difficulty=${selectedDifficulty}&type=multiple`);
		const data = await response.json();

		questions = data.results

		questions.forEach((question) => {
			let answers = [];

			question.incorrect_answers.forEach((answer) => {
				answers.push({ answer: answer, correct: false})
			});
			answers.push({ answer: question.correct_answer, correct: true });

			let allQuestions = {
				question: question.question,
				answers: _.shuffle(answers),
			}
			
			displayQuestions.push(allQuestions)
		})
		displayQuestions = displayQuestions
	}

	function incorrectAnswer() {
		incorrect++
	}

	function correctAnswer() {
		correct++
	}

</script>


<main>
	<h1>{name}</h1>

	<h2>In order to begin, please select a category and a difficulty level. Then click "Start Quiz"</h2>
	<h4>Note: In order to receive an accurate score, please only select an answer for each question once</h4>
	
	<div class="categoryContainer">
		<label for="category">Category</label>
		<select bind:value="{selectedCategory}" name="category" id="category">
			{#each categories as category}
			<option value="{category.id}">{category.name}</option>
			{/each}
		</select>
	</div>

	<div class="difficultyContainer">
		<label for="difficulty">Difficulty</label>
		<select bind:value="{selectedDifficulty}" name="difficulty" id="difficulty">
			<option value="easy">Easy</option>
			<option value="medium">Medium</option>
			<option value="hard">Hard</option>
		</select>
	</div>

	<button on:click={getQuestions}>Start Quiz</button>

	<div class="quizContainer">
		{#each displayQuestions as question}
		<h3>{@html _.unescape(question.question)}</h3>

			<div class="answerContainer">
				{#each question.answers as answer}

				{#if answer.correct === false}
				<button on:click={incorrectAnswer} class="answerButton">{@html _.unescape(answer.answer)}</button>
				{:else}
				<button on:click={correctAnswer} class="answerButton">{@html _.unescape(answer.answer)}</button>
				{/if}

				{/each}
			</div>
			
		{/each}
	</div>

	<div class="score">
		<p class="correctScore">Correct: { correct }</p>
		<p class="incorrectScore">Incorrect: { incorrect }</p>
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
		color: #FF5A5F;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	h4 {
		color: #FF5A5F;
	}
	.categoryContainer, .difficultyContainer {
		margin: 15px;
	}
	label {
		margin-bottom: 5px;
		font-weight: bold;
	}
	select {
		width: 200px;
	}
	button {
		cursor: pointer;
		border-color: #FF5A5F;
		background-color: #FFF;
		color: #FF5A5F;
		padding: 10px 15px;
		transition: all .4s;
	}
	.quizContainer {
		margin: 15px 0 15px 0;
	}
	.quizContainer h3 {
		margin-bottom: 5px;
	}
	.answerContainer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	button:hover {
		background-color: #FF5A5F;
		color: #FFF;
	}
	.answerButton {
		border-color: #000;
		color: #000;
		width: 200px;
	}
	.answerButton:hover {
		background-color: #000;
		color: #FFF;
	}
	.correctScore {
		color: #1DB954;
	}
	.incorrectScore {
		color: #FF0000;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>