<template>
	<flux-grid
		:slider="slider"
		:num-rows="numRows"
		:num-cols="numCols"
		:images="images"
		:tile-css="tileCss"
		ref="grid">
	</flux-grid>
</template>

<script>
	import FluxGrid from '@/components/FluxGrid.vue';

	let vf, currentImage, nextImage;

	export default {
		name: 'transitionRound1',

		components: {
			FluxGrid,
		},

		data: () => ({
			numRows: 1,
			numCols: 1,
			tileDuration: 800,
			totalDuration: 0,
			easing: 'ease-out',
			tileDelay: 150,
			tileCss: {},
			images: {
				front: {},
				back: {},
			},
		}),

		props: {
			slider: {
				type: Object,
				required: true,
			},
		},

		computed: {
			grid: function() {
				return this.$refs.grid;
			},
		},

		created() {
			vf = this.slider;
			currentImage = vf.Images.current;
			nextImage = vf.Images.next;

			let divider = vf.size.width / 6;

			vf.Transitions.setOptions(this, {
				numRows: Math.floor(vf.size.height / divider),
				numCols: Math.floor(vf.size.width / divider),
			});

			this.totalDuration = this.tileDelay * (this.numRows > this.numCols? this.numRows : this.numCols) * 2 + this.tileDelay;

			this.images.front = currentImage.getProperties();
			this.images.back = nextImage.getProperties();
		},

		mounted() {
			currentImage.hide();
			nextImage.hide();

			this.grid.setCss({
				perspective: '800px',
			});

			this.grid.transform((tile, i) => {
				tile.setCss({
					transition: 'all '+ this.tileDuration +'ms '+ this.easing +' '+ this.getDelay(i) +'ms',
				});

				tile.turn('back', this.direction);
			});
		},

		destroyed() {
			nextImage.show();
		},

		methods: {
			getDelay(i) {
				let row = this.grid.getRowNumber(i);
				let col = this.grid.getColNumber(i);
				let delay = col + row;

				if (this.direction === 'left')
					delay = this.numRows + this.numCols - delay - 1;

				return delay * this.tileDelay;
			},
		},
	};
</script>
