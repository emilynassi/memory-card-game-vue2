<template>
	<div id="app" class="s-main">
		<main-header :reset="resetDeck" />
		<section class="s-stats">
			<div>Number of Moves: {{ numberofMoves }}</div>
			<div class="divider">|</div>
			<div>Number of Matches: {{ numberofMatches }}</div>
			<div class="divider">|</div>

			<div>Clock: {{ minutes }}:{{ seconds }}</div>
		</section>
		<section
			class="s-card-container"
			:class="{ 'pointer-none': activeMatchValue }"
		>
			<card
				v-for="card in cards"
				:key="card.id"
				:card="card"
				:class="{
					'is-flipped': card.isFlipped,
					'is-matched': card.isMatched,
				}"
				:flip="flipCard"
			/>
		</section>
		<div class="s-github">
			<a
				href="https://github.com/emilynassi/memory-card-game-vue2"
				aria-label="View on Github"
				target="_blank"
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="24"
					height="24"
					viewBox="0 0 24 24"
				>
					<path
						d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"
					/>
				</svg>
			</a>
		</div>
	</div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
import MainHeader from '@/components/MainHeader.vue';
import Card from '@/components/Card.vue';

export default {
	name: 'App',
	components: {
		MainHeader,
		Card,
	},
	data: () => ({
		defaultCards: [
			{ emoji: '🍹', value: 'drink' },
			{ emoji: '🌴', value: 'palm' },
			{ emoji: '🏄‍♂️', value: 'green surfer' },
			{ emoji: '🐠', value: 'fish' },
			{ emoji: '🩴', value: 'flip flop' },
			{ emoji: '🌊', value: 'wave' },
			{ emoji: '🏄‍♀️', value: 'pink surfer' },
			{ emoji: '🦩', value: 'flamingo' },
			{ emoji: '🏖', value: 'beach' },
			{ emoji: '😎', value: 'smile with sunglasses' },
			{ emoji: '☀️', value: 'sun' },
			{ emoji: '⛵️', value: 'sailboat' },
		],
		cards: [],
		activeMatchArray: [],
		activeMatchValue: null,
		numberofMatches: 0,
		numberofMoves: 0,
		gameClock: null,
		timer: {
			minutes: 0,
			seconds: 0,
		},
	}),
	computed: {
		seconds() {
			return this.timer.seconds < 10
				? `0${this.timer.seconds}`
				: this.timer.seconds;
		},
		minutes() {
			return this.timer.minutes < 10
				? `0${this.timer.minutes}`
				: this.timer.minutes;
		},
	},
	created() {
		this.setCards();
	},
	methods: {
		flipCard(id, value, card) {
			if (!this.gameClock) {
				this.startGameClock();
			}
			this.cards = this.cards.map((card) => ({
				...card,
				isFlipped: card.id === id ? true : card.isFlipped,
			}));
			if (this.activeMatchArray.length < 2) {
				this.activeMatchArray.push({ id, value });
			}
			if (this.activeMatchArray.length === 2) {
				this.activeMatchValue = true;
				this.checkMatch(value);
			}
		},
		setCards() {
			this.cards = [...this.defaultCards, ...this.defaultCards]
				.sort(() => 0.5 - Math.random())
				.map((card) => ({
					...card,
					id: uuidv4(),
					isFlipped: false,
					isMatched: false,
				}));
		},
		checkMatch(value) {
			this.numberofMoves += 1;
			if (this.activeMatchArray[0].value === this.activeMatchArray[1].value) {
				window.setTimeout(() => {
					this.cards = this.cards.map((card) => ({
						...card,
						isMatched: value === card.value ? true : card.isMatched,
					}));
				}, 1500);
				this.numberofMatches += 1;
				this.activeMatchArray = [];
				this.activeMatchValue = false;
			} else {
				this.cards.forEach((card) => {
					this.activeMatchArray = [];
					window.setTimeout(() => {
						card.isFlipped = false;
						this.activeMatchValue = false;
					}, 1500);
				});
			}
		},
		startGameClock() {
			let countDownDate = new Date();
			this.gameClock = setInterval(() => {
				let now = new Date().getTime();
				let distance = now - countDownDate.getTime();
				this.timer.minutes = Math.floor(
					(distance % (1000 * 60 * 60)) / (1000 * 60),
				);
				this.timer.seconds = Math.floor((distance % (1000 * 60)) / 1000);
			}, 1000);
		},
		clearGameClock() {
			clearInterval(this.gameClock);
			this.gameClock = null;
			this.timer.seconds = 0;
			this.timer.minutes = 0;
		},
		resetDeck() {
			//reset card deck
			this.setCards();
			//clear out any remaing values
			this.numberofMoves = 0;
			this.numberofMatches = 0;
			this.activeMatchValue = null;
			this.activeMatchArray = [];
			this.clearGameClock();
		},
	},
};
</script>

<style lang="scss">
.s-card-container {
	max-width: 1280px;
	display: grid;
	justify-content: center;
	grid-template-columns: repeat(8, minmax(min-content, 125px));
	grid-auto-flow: dense;
	grid-gap: 12px;
	justify-items: center;
	grid-auto-rows: minmax(min-content, 1fr);
	margin: 32px auto 0;
}
.pointer-none {
	pointer-events: none;
}

.s-stats {
	display: flex;
	justify-content: center;
	align-items: center;
	margin-top: 24px;
	div.divider {
		margin: 0 6px;
		font-weight: 900;
	}
}
.s-github {
	padding: 0 24px;
	margin: 48px auto;
	text-align: center;
	svg {
		transition: 0.15s;
	}
	a {
		&:hover {
			svg {
				fill: $bright-pink;
			}
		}
	}
}

@media screen and (max-width: 1117px) {
	.s-card-container {
		grid-template-columns: repeat(4, minmax(min-content, 75px));
	}
}
</style>
