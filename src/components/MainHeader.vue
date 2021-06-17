<template>
	<header class="c-header">
		<div class="c-header__inner">
			<h1 class="c-header__title">
				<img
					class="c-header__title-img"
					src="@/assets/images/palmtree.gif"
					alt="palm tree"
				/>
				Match Game
			</h1>

			<div class="c-header__stats">
				<div>Number of Moves: {{ moves }}</div>
				<div class="divider">|</div>
				<div>Number of Matches: {{ matches }}</div>
				<div class="divider">|</div>

				<div>Clock: {{ minutes }}:{{ seconds }}</div>
			</div>
			<button class="btn btn-primary" @click="reset">Reset</button>
		</div>
	</header>
</template>

<script>
export default {
	name: 'MainHeader',
	props: {
		moves: {
			type: Number,
			default: 0,
		},
		matches: {
			type: Number,
			default: 0,
		},
		timer: {
			type: Object,
			default: null,
		},
		reset: {
			type: Function,
			required: true,
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
.c-header {
	background-color: $blue;
	height: 80px;
	&__inner {
		max-width: 1280px;
		margin: 0 auto;
		height: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	&__title {
		margin: 0;
		display: flex;
		align-items: center;
		&-img {
			height: 80px;
			width: 80px;
			margin-right: 12px;
		}
	}
	&__stats {
		display: flex;
		justify-content: space-evenly;
		align-items: center;
		div.divider {
			margin: 0 6px;
			font-weight: 900;
		}
	}
}
</style>
