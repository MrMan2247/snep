<template>
	<div class="snep">
		<div class="trigger" @click="show">
			<slot></slot>
		</div>
		<div :class="'image-container ' + containerClass">
			<div class="btns" v-if="!plain">
				<div class="btn" @click="rotate(90)">&#x21bb;</div>
				<div class="btn" @click="rotate(-90)">&#x21ba;</div>
			</div>
			<div class="image" :style="imageStyle" @click="hide"></div>
		</div>
	</div>
</template>

<script>
	export default {
		props: ['image', 'plain'],
		data () {
			return {
				shown: null,
				state: {
					rotation: 0
				}
			}
		},
		computed: {
			background () {
				return 'background-image: url(' + this.image + ');';
			},
			imageStyle () {
				var style = this.background + ";transform: translate(-50%, -50%)";
				if (this.state.rotation) {
					style += " rotate(" + this.state.rotation + "deg)";
				}
				return style;
			},
			containerClass () {
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
			},
			rotate ( amount ) {
				this.state.rotation = (this.state.rotation + amount) % 360;
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

		* { cursor: pointer; }

		.image-container {
			position: fixed;
			top: 0;
			left: 0;
			width: 100vw;
			height: 100vh;
			background: rgba(20, 20, 20, 0.75);
			opacity: 0;
			z-index: $send-to-back;

			.btns {
				position: relative;
				display: flex;
				width: 100vw;
				max-width: 320px;
				margin: 0 auto;
				padding: 10px;
				z-index: $send-to-front;

				.btn {
					flex: 1 1 40px;
					color: rgba(255,255,255,1);
					font-size: 1.3rem;
					text-align: center;

					&:hover {
						color: rgba(255,255,255,0.8);
					}
				}
			}

			.image {
				width: 80vw;
				height: 80vh;
				background-size: contain;
				background-repeat: no-repeat;
				background-position: center center;
				position: absolute;
				left: 50%;
				top: 50%;
				transform: translate(-50%, calc(-50% - 100px));
				transition: 0.5s transform;
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

			&.hidden  {
				animation-name: hide;

				.image { transform: translate(-50%, calc(-50% - 100px)); }
			}
			&.shown  {
				animation-name: show;

				.image { transform: translate(-50%, -50%); }
			}
		}

		.trigger {
			height: 100%;
			width: 100%;
		}
	}
</style>