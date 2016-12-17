<template>
	<div class="snep">
		<div class="trigger" @click="show">
			<slot></slot>
		</div>
		<div :class="'box-container' + (shown ? ' shown' : ' hidden')" @click="hide">
			<div :style="background"></div>
		</div>
	</div>
</template>

<script>
	export default {
		props: ['image'],
		data () {
			return {
				shown: false
			}
		},
		computed: {
			background () {
				return 'background-image: url(' + this.image + ');';
			}
		},
		methods: {
			show () {
				this.shown = true;
			},
			hide () {
				this.shown = false;
			}
		},
		mounted () {
			// Preloader
			(new Image()).src = this.image;
		}
	}
</script>

<style lang="scss">
	.snep {
		position: relative;
		display: inline-block;
		cursor: pointer;

		.box-container {
			position: fixed;
			top: 0;
			left: 0;
			width: 100vw;
			height: 100vh;
			background: rgba(20, 20, 20, 0.75);
			z-index: 9999999;

			>div {
				width: 80vw;
				height: 80vh;
				background-size: contain;
				background-repeat: no-repeat;
				background-position: center center;
				position: absolute;
				left: 50%;
				top: 50%;
				transform: translate(-50%, -50%);
			}

			&.hidden { display: none;  }
			&.shown  { display: block; }
		}

		.trigger {
			height: 100%;
			width: 100%;
		}
	}
</style>