<template>
	<div id="app">
		<main-header />
		<section class="s-card-container">
			<card
				v-for="card in cards"
				:key="card.id"
				:card="card"
				:class="{ 'is-flipped': card.isFlipped }"
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
			{
				emoji: '🍹',
				value: 'drink',
			},
			{
				emoji: '🌴',
				value: 'palm',
			},
			{
				emoji: '🏄‍♂️',
				value: 'green surfer',
			},
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
		activeMatchValue: null,
	}),
	created() {
		this.setCards();
	},
	methods: {
		flipCard(id, value) {
			this.cards = this.cards.map((card) => ({
				...card,
				isFlipped: card.id === id ? true : card.isFlipped,
			}));
			// card.isFlipped = !card.isFlipped;
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
	},
};
</script>

<style lang="scss">
@import '@/assets/global.scss';

.s-card-container {
	max-width: 1000px;
	display: grid;
	grid-template-columns: repeat(8, 1fr);
	grid-gap: 12px;
	justify-items: center;

	//   max-width: 1000px;
	// display: grid;
	// grid-template-columns: repeat(8, minmax(min-content, 1fr));
	// grid-auto-flow: dense;
	// grid-gap: 12px;
	// justify-items: center;
	// grid-auto-rows: minmax(100px, 1fr);
	// margin: 0 auto;
}
</style>
