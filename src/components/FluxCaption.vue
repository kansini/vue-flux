<template>
	<transition name="fade">
		<div v-if="caption" class="flux-caption">{{ caption }}</div>
	</transition>
</template>

<script>
	export default {
		name: 'FluxCaption',

		props: {
			slider: {
				type: Object,
			},
		},

		computed: {
			vf: function() {
				if (this.slider)
					return this.slider;

				if (this.$parent.$options.name === 'VueFlux')
					return this.$parent;

				console.warn('slider not referenced, check https://github.com/deulos/vue-flux/wiki/FluxCaption for help');

				return undefined;
			},

			caption: function() {
				if (!this.vf)
					return '';

				if (this.vf.loaded === false)
					return '';

				if (this.vf.Transitions.current !== undefined)
					return '';

				let currentImage = this.vf.Images.current;

				if (currentImage === undefined)
					return '';

				if (this.captions[currentImage.index] === undefined)
					return '';

				return this.captions[currentImage.index];
			},

			captions: function() {
				return this.vf? this.vf.captions : {};
			},
		},
	};
</script>

<style lang="scss">
	.vue-flux .flux-caption {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		padding: 8px;
		color: white;
		text-align: center;
		background-color: rgba(0, 0, 0, 0.65);
		z-index: 100;

		&.fade-enter, &.fade-leave-to {
			opacity: 0;
		}

		&.fade-enter-active, &.fade-leave-active {
			transition: opacity 0.3s ease-in;
		}
	}
</style>
