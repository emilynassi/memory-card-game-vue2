<template>
	<div class="component c-card" @click="flip(card.id, card.value, card)">
		<div class="c-card__inner">
			<div class="c-card__inner-front"></div>
			<div class="c-card__inner-back">
				<span :aria-label="card.value">{{ card.emoji }}</span>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Card',
	props: {
		card: {
			type: Object,
			required: true,
		},
		flip: {
			type: Function,
			required: true,
		},
	},
};
</script>

<style lang="scss" scoped>
.c-card {
	width: 125px;
	height: 125px;
	cursor: pointer;
	transition: 0.5s;

	&__inner {
		height: 100%;
		width: 100%;
		position: relative;

		&-front,
		&-back {
			backface-visibility: hidden;
			transition: 0.5s;
			transform-style: preserve-3d;
			top: 0;
			left: 0;
			height: 100%;
			width: 100%;
			position: absolute;
			border-radius: 4px;
		}
		&-front {
			background-color: $bright-pink;
		}
		&-back {
			-ms-transform: rotateY(-180deg);
			transform: rotateY(-180deg);
			position: absolute;
			display: flex;
			justify-content: center;
			align-items: center;
			span {
				font-size: 5rem;
			}
		}
	}

	&.is-flipped {
		.c-card__inner {
			&-front {
				transform: rotateY(180deg);
			}
			&-back {
				background-color: $light-pink;
				transform: rotateY(0deg);
			}
		}
	}
	&.is-matched {
		opacity: 0;
	}
}
</style>
