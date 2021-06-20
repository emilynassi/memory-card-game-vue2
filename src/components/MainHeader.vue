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

			<button class="btn btn-primary c-header__reset" @click="reset">
				Reset
			</button>
		</div>
	</header>
</template>

<script>
export default {
	name: 'MainHeader',
	props: {
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
		padding: 0 12px;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	&__title {
		margin: 0 0 0 auto;
		display: flex;
		align-items: center;
		&-img {
			height: 80px;
			width: 80px;
			margin-right: 12px;
		}
	}
	&__reset {
		margin-left: auto;
	}
}
</style>
