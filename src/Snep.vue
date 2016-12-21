<template>
	<div class="snep">
		<div class="trigger" @click="show">
			<slot></slot>
		</div>
		<div :class="'box-container ' + boxClass" @click="hide">
			<div :style="background"></div>
		</div>
	</div>
</template>

<script>
	export default {
		props: ['image'],
		data () {
			return {
				shown: null,
			}
		},
		computed: {
			background () {
				return 'background-image: url(' + this.image + ');';
			},
			boxClass () {
				if (this.shown !== null) {
					return (this.shown ? 'shown' : 'hidden');
				} else {
					return 'initalized';
				}
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
		mounted () { (new Image()).src = this.image; }
	}
</script>

<style lang="scss">
	$send-to-back: -9999999;
	$send-to-front: 9999999;

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
			opacity: 0;
			z-index: $send-to-back;

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

			@keyframes hide {
				0% {
					z-index: $send-to-front;
					opacity: 1;
				}
				99% {
					z-index: $send-to-front;
					opacity: 0;
				}
				100% {
					z-index: $send-to-back;
					opacity: 0;
				}
			}

			@keyframes show {
				0% {
					z-index: $send-to-back;
					opacity: 0;
				}
				1% {
					z-index: $send-to-front;
					opacity: 0;
				}
				100% {
					z-index: $send-to-front;
					opacity: 1;
				}
			}

			animation-duration: 0.5s;
			animation-fill-mode: forwards;

			&.hidden  { animation-name: hide; }
			&.shown  { animation-name: show; }
		}

		.trigger {
			height: 100%;
			width: 100%;
		}
	}
</style>