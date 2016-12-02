<template>
	<div class="snep">
		<div class="trigger" @click="show">
			<slot></slot>
		</div>
		<div :class="'box' + (shown ? ' shown' : ' hidden')">
			<div :style="image"></div>
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
		methods: {
			show () {
				this.shown = true;
			},
			hide () {
				this.shown = false;
			}
		}
	}
</script>

<style lang="sass">
	.box {
		//omg the lag is so aggravating, I gotta get some sleep, it's getting late :(
		&.hidden { display: none;  }
		&.shown  { display: block; }
	}
</style>