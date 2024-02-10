<script setup>
	import { ref, onMounted, computed } from 'vue';

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

	const winPercentage = computed(() => {
		const total = wins.value + losses.value + draws.value;
		return total ? Math.round((wins.value / total) * 100) : 0;
	});

	const play = (choice) => {
		const choices = ['rock', 'paper', 'scissors'];
		const random = Math.floor(Math.random() * choices.length);

		computerChoice.value = choices[random];
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

		saveGame();
	};

	const saveGame = () => {
		localStorage.setItem('wins', wins.value);
		localStorage.setItem('losses', losses.value);
		localStorage.setItem('draws', draws.value);
	};

	const loadGame = () => {
		wins.value = localStorage.getItem('wins');
		losses.value = localStorage.getItem('losses');
		draws.value = localStorage.getItem('draws');
	};

	const resetRound = () => {
		userChoice.value = null;
		computerChoice.value = null;
		verdict.value = null;
	};

	onMounted(() => {
		loadGame();

		window.addEventListener('keypress', (event) => {
			if (event.key === 'r') {
				resetRound();
			}
		});
	});
</script>

<template>
	<div
		class="flex flex-col min-h-screen text-center bg-slate-900 text-slate-400"
	>
		<header class="container p-10 mx-auto">
			<h1 class="text-4xl underline underline-offset-8">
				Rock Paper Scissors
			</h1>
		</header>
		<main class="container flex-1 p-10 mx-auto">
			<div
				v-if="userChoice === null"
				class="flex items-center justify-center -mx-10"
			>
				<button
					@click="play('rock')"
					class="w-64 p-5 mx-10 duration-300 rounded-xl bg-slate-400 shadow-slate-400 hover:bg-green-500"
				>
					<img src="./assets/rock.svg" alt="rock" class="w-full" />
				</button>
				<button
					@click="play('rock')"
					class="w-64 p-5 mx-10 duration-300 rounded-xl bg-slate-400 shadow-slate-400 hover:bg-pink-500"
				>
					<img src="./assets/paper.svg" alt="papers" class="w-full" />
				</button>
				<button
					@click="play('rock')"
					class="w-64 p-5 mx-10 duration-300 rounded-xl bg-slate-400 shadow-slate-400 hover:bg-blue-500"
				>
					<img
						src="./assets/scissors.svg"
						alt="scissors"
						class="w-full"
					/>
				</button>
			</div>
			<div v-else>
				<h2 class="mb-4 text-3xl">
					You picked
					<span class="text-green-500">{{ userChoice }}</span>
				</h2>
				<h2 class="mb-4 text-3xl">
					The computer picked
					<span class="text-yellow-500">{{ computerChoice }}</span>
				</h2>
				<h2 class="mb-12 text-6xl">
					{{ verdict }}
				</h2>
				<button
					@click="resetRound"
					class="px-4 py-2 text-lg text-[#262626] duration-300 rounded bg-slate-400 hover:bg-pink-500"
				>Reset</button>
			</div>
		</main>
	</div>
</template>
