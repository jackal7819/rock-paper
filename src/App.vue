<script setup>
	import { ref, mounted, computed } from 'vue';

	const wins = ref(0);
	const losses = ref(0);
	const draws = ref(0);

	const userChoice = ref(null);
	const computerChoice = ref(null);
	const verdict = ref(null);

	const outcomes = {
		rock: {
			rock: 'draw',
			paper: 'loss',
			scissors: 'win',
		},
		paper: {
			rock: 'win',
			paper: 'draw',
			scissors: 'loss',
		},
		scissors: {
			rock: 'loss',
			paper: 'win',
			scissors: 'draw',
		},
	};

	const play = (choice) => {
		const choices = ['rock', 'paper', 'scissors'];
		const random = Math.floor(Math.random() * choices.length);
		
		computerChoice.value = Math.floor(Math.random() * choices.length)		
		userChoice.value = choice;
		verdict.value = choices[random];

		const outcome = outcomes[choice][verdict.value];
		
		if (outcome === 'win') {
			wins.value++;
			verdict.value = 'You win!';
		} else if (outcome === 'loss') {
			losses.value++;
			verdict.value = 'You lose!';
		} else {
			draws.value++;
			verdict.value = 'It is a draw!';
		}

		saveGame()
	};

	const saveGame = () => {
		localStorage.setItem('wins', wins.value);
		localStorage.setItem('losses', losses.value);
		localStorage.setItem('draws', draws.value);
	};
	
</script>

<template>
	<main>
		<h1 class="text-4xl underline">Rock Paper Scissors</h1>
	</main>
</template>
