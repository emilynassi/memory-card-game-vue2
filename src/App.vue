<template>
	<div id="app">
		<main-header
			:matches="numberofMatches"
			:moves="numberofMoves"
			:reset="resetDeck"
		/>
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
	}),
	created() {
		this.setCards();
	},
	methods: {
		flipCard(id, value, card) {
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
		resetDeck() {
			//reset card deck
			this.setCards();
			//clear out any remaing values
			this.numberofMoves = 0;
			this.numberofMatches = 0;
			this.activeMatchValue = false;
			this.activeMatchArray = false;
		},
	},
};
</script>

<style lang="scss">
.s-card-container {
	max-width: 1280px;
	display: grid;
	grid-template-columns: repeat(8, minmax(min-content, 1fr));
	grid-auto-flow: dense;
	grid-gap: 12px;
	justify-items: center;
	grid-auto-rows: minmax(100px, 1fr);
	margin: 0 auto;
}
.pointer-none {
	pointer-events: none;
}
</style>
