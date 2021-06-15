<template>
	<header class="c-header">
		Number of Moves: {{ moves }} <br />
		Number of Matches: {{ matches }} <br />
		Clock: {{ gameClock }}
		<button @click="reset">Reset</button>
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
		gameClock: {
			type: Number,
			default: 0,
		},
		reset: {
			type: Function,
			required: true,
		},
	},
};
</script>

<style lang="scss" scoped>
.c-header {
	background-color: $blue;
	height: 80px;
}
</style>
