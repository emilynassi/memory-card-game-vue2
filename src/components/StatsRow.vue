<template>
	<section class="stats">
		<div>Number of Moves: {{ moves }}</div>
		<div class="divider">|</div>
		<div>Number of Matches: {{ matches }}</div>
		<div class="divider">|</div>

		<div>Clock: {{ minutes }}:{{ seconds }}</div>
	</section>
</template>

<script>
export default {
	name: 'StatsRow',
	props: {
		moves: {
			type: Number,
			default: 0,
		},
		numberOfMatches: {
			type: Number,
			default: 0,
		},
		// eslint-disable-next-line vue/require-default-prop
		timer: {
			type: Object,
		},
	},
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
};
</script>

<style lang="scss" scoped>
.stats {
	display: flex;
	justify-content: center;
	align-items: center;
	margin-top: 24px;
	div.divider {
		margin: 0 6px;
		font-weight: 900;
	}
}
</style>
