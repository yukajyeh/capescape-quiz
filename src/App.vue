<template>
	<main class="app">
		<h1>Techival CapEscape Quiz</h1>

		<!-- <section class="loading">
			<p>You ready to start?</p>
		</section> -->

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
						v-model="getCurrentQuestion.selected" :disabled="getCurrentQuestion.selected"
						@change="setAnswer" />
					<span>{{ option }}</span>
				</label>

				<div class="bbox">
					<button @click="nextQuestion" :disabled="!getCurrentQuestion.selected">
						{{
						getCurrentQuestion.index == questions.length - 1
						? 'Finish'
						: getCurrentQuestion.selected == null
						? 'Select an option'
						: 'Next question'
						}}
					</button>
				</div>

			</div>


		</section>

		<section v-else>
			<h2>You made it!</h2>
			<p>Your score is {{ score }}/{{ questions.length }}</p>
			<div class="bbox">
				<button @click="resetQuiz">Restart</button>
			</div>
		</section>
		<footer><img src='../public/capgemini-logo.png' alt='capgemini-logo' class='logo' /></footer>
	</main>
</template>

<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
	question: "What does CPU stand for?",
	answer: 0,
	options: [
		'Central Processing Unit',
		'Community Programming Utility',
		'Common Popular Universe'
	],
	selected: null
  },
  {
	question: "Where was the world wide web invented?",
	answer: 2,
	options: [
		'United States',
		'Germany',
		'Switzerland'
	],
	selected: null
  },
  {
	question: "Who is the organizer of Techival?",
	answer: 1,
	options: [
		'Yahoo',
		'Capgemini',
		'Capricon'
	],
	selected: null
  },
  {
	question: "Which of these is NOT an early computer?",
	answer: 2,
	options: [
		'ENIAC',
		'UNIVAC',
		'NASA'
	],
	selected: null
   },
   {
	   question: "Which technology was named after the nickname of Denmark and Norway's 10th century king, Harald Gormsson?",
		answer: 1,
		options: [
			'EMAIL',
			'Bluetooth',
			'APPLE'
		],
		selected: null
	},
	{
		question: "Which company developed the Mac Operating System?",
		answer: 0,
		options: [
			'Apple',
			'IBM',
			'Samsung'
		],
		selected: null
	},
	{
		question: "Which of the following is NOT a programming language?",
		answer: 2,
		options: [
			'Javascript',
			'Java',
			'J.A.R.V.I.S'
		],
		selected: null
	},
	{
		question: "What does TL:DR stand for?",
		answer: 0,
		options: [
			"Too Long, Didn't read",
			"Talk Later, Don't bother",
			'Take left, down right'
		],
		selected: null
	},
	{
		question: "What happens if a CD spins too fast?",
		answer: 2,
		options: [
			"It might seperate the disc's layers",
			"It might melt",
			'It might explode'
		],
		selected: null
	},
	{
		question: "What is the name of this escape room?",
		answer: 0,
		options: [
			"CapEscape",
			"Escape Capgemini",
			'Techivalscape'
		],
		selected: null
	},
	{
		question: "What is the origin of the word 'spam' in the context of email spam?",
		answer: 1,
		options: [
			"Poor developers sick of eating the same meal",
			"A Monty Python skit from the 70s",
			"It's an acronym for 'spontaneously persistent advertising message'"
		],
		selected: null
	},
	{
		question: "Before becoming widely recognized the main character of Super Mario Bros., what was Mario named?",
		answer: 2,
		options: [
			"Bouncing Carpenter",
			"Hammer Jump",
			"Jumpman"
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

const setAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}

const nextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}

	quizCompleted.value = true
}

const resetQuiz = () => {
	quizCompleted.value = false;
	currentQuestion.value = 0;
	questions.value.map((q) => {
		q.selected = null
	})
}
</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Inconsolata:wght@300;400&display=swap');

* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Inconsolata', monospace;
}

body {
	background-color: #145DA0;
	color: #FFF;
}

.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
}

h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
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

.quiz {
	background-color: #2E8BC0;
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
	color: #FFF;
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
	background-color: #0C2D48;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
}

.option:hover {
	background-color: #60A3D9;
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

.bbox {
	display:flex;
	justify-content: center;
	align-items: center;
}

button {
	outline: none;
	border: none;
	cursor: pointer;
	padding: 1rem;
	margin: 1rem;
	background-color: #2cce7d;
	color: #2d213f;
	font-weight: 700;
	text-transform: uppercase;
	font-size: 1.2rem;
	border-radius: 0.5rem;
}

button:disabled {
	opacity: 0.5;
}

footer {
	position: absolute;
	bottom: 0;
	top:50;
	left:50;
	margin-bottom: 1rem;
}

.logo {
	max-width: 10em;
}

</style>
