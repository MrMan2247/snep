<template>
	<div class="snep">
		<div class="trigger" @click="show">
			<slot></slot>
		</div>
		<div :class="'image-container ' + containerClass">
			<div class="btns" v-if="plain !== undefined">
				<div class="btn" @click="rotate(45)">&#x21bb;</div>
				<div class="btn" @click="scale(0.1)">+</div>
				<div :class="'btn reset' + (isReset ? '' : ' ready')" @click="reset">&#x2205;</div>
				<div class="btn" @click="scale(-0.1)">-</div>
				<div class="btn" @click="rotate(-45)">&#x21ba;</div>
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
					rotation: 0,
					scale: 1
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
				if (this.state.scale !== 1) {
					style += " scale(" + this.state.scale + ")";
				}
				return style;
			},
			containerClass () {
				if (this.shown !== null) {
					return (this.shown ? 'shown' : 'hidden');
				} else {
					return 'initalized';
				}
			},
			isReset () {
				return JSON.stringify(this.state) === JSON.stringify({
					rotation: 0,
					scale: 1
				})
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
			},
			scale ( amount ) {
				this.state.scale = this.state.scale + amount;
			},
			reset () {
				this.state = {
					rotation: 0,
					scale: 1
				}
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

				* { user-select: none; }

				.btn {
					flex: 1 1 40px;
					color: rgba(205,205,205,0.6);
					font-size: 1.3rem;
					text-align: center;
					text-shadow: -1px 1px 0 rgba(15,15,15,0.75);
					transition: all 0.2s ease;

					&:hover {
						color: rgba(205,205,205,0.8);
						text-shadow: -1px 1px 0 rgba(1,1,1,1);
					}

					&.reset {
						flex: 0 0 0;
						opacity: 0;
						overflow: hidden;
						line-height: 29px;
						transition: 0.4s all ease;

						&.ready {
							flex: 1 1 40px;
							opacity: 1;
						}
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

			&.initalized {
				.image { transform: translate(-50%, calc(-50% - 100px)) !important; }
			}

			&.hidden {
				animation-name: hide;

				.image { transform: translate(-50%, calc(-50% - 100px)) !important; }
			}

			&.shown {
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