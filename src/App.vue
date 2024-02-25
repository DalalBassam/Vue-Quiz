<script setup>
import { ref, computed } from 'vue'

const questions = ref([
	{
		question: 'Which of the directives below provide a two-way binding?',
		answer: 0,
		options: [
			'v-model',
			'v-inline',
			'v-on'
		],
		selected: null
	},
	{
		question: 'What is the purpose of the computed property in Vue.js?',
		answer: 2,
		options: [
			'To define asynchronous operations',
			'To interact with the DOM directly',
			'To calculate and return a value based on reactive dependencies'
		],
		selected: null
	},
	{

		question: 'How can you pass data from a parent to a child component?',
		answer: 1,
		options: [
			'Using the $emit method',
			'Using props',
			'Using the $parent property'
		],
		selected: null
	}
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})

const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})

const SetAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}

const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}

	quizCompleted.value = true
}
</script>

<template>
	<main class="app">
		<h1>Vue Quiz</h1>

		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Score {{ score }}/{{ questions.length }}</span>
			</div>

			<div class="options">
				<label v-for="(option, index) in getCurrentQuestion.options" :for="'option' + index" :class="`option ${getCurrentQuestion.selected == index
					? index == getCurrentQuestion.answer
						? 'correct'
						: 'wrong'
					: ''
					} ${getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
						? 'disabled'
						: ''
					}`">
					<input type="radio" :id="'option' + index" :name="getCurrentQuestion.index" :value="index"
						v-model="getCurrentQuestion.selected" :disabled="getCurrentQuestion.selected" @change="SetAnswer" />
					<span>{{ option }}</span>
				</label>
			</div>

			<button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
				{{
					getCurrentQuestion.index == questions.length - 1
					? 'Finish'
					: getCurrentQuestion.selected == null
						? 'Select an option'
						: 'Next question'
				}}
			</button>
		</section>

		<section v-else>
			<h2>You have finished the quiz!</h2>
			<p>Your score is {{ score }}/{{ questions.length }}</p>
		</section>
	</main>
</template>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Montserrat', sans-serif;
}

body {
	background-color: #d1c8c885;
}

.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 4rem;
	height: 100vh;

}

h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
}

.quiz {
	background-color: #FFF;
	padding: 1rem;
	width: 100%;
	max-width: 640px;
}

.quiz-info {
	display: flex;
	justify-content: space-between;
	margin-bottom: 1rem;
}

.quiz-info .question {
	color: #202020;
	font-size: 1.25rem;
}

.quiz-info.score {
	color: #FFF;
	font-size: 1.25rem;
}

.options {
	margin-bottom: 1rem;
}

.option {
	padding: 1rem;
	display: block;
	background-color: #f0f0f1;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
}

.option.correct {
	background-color: #2cce7d;
}

.option.wrong {
	background-color: #ff5a5f;
}

.option:last-of-type {
	margin-bottom: 0;
}

.option.disabled {
	opacity: 0.5;
}

.option input {
	display: none;
}

button {
	appearance: none;
	outline: none;
	border: none;
	cursor: pointer;
	float: right;
	padding: 0.5rem 1rem;
	background-color: #2cce7d;
	color: #0a0a0a;
	font-size: 1.2rem;
	border-radius: 0.5rem;
}

button:disabled {
	opacity: 0.5;
}

h2 {
	font-size: 2rem;
	margin-bottom: 2rem;
	text-align: center;
}

p {
	color: #8F8F8F;
	font-size: 1.5rem;
	text-align: center;
}
</style>
