<template>
	<div class="home">
		<div class="settings">
			<div class="outer-container">
				<div class="input-group">
					<label for="cardCount">How many cards?</label>
					<input
						type="text"
						id="cardCount"
						v-model="cardCount"
					>
				</div>
				<div class="input-group">
					<label for="theme">What kind of pictures?</label>
					<input
						type="text"
						id="theme"
					>
				</div>

				<AppButton @click.native="startGame()">Start Game</AppButton>
			</div>
		</div>

		<div class="card-grid">
			<div class="outer-container">
				<div
					class="card-container"
					:style="gridStyles"
				>
					<div
						v-for="(card, index) in cards"
						:key="index"
						class="card"
					>
						<img :src="`https://farm${card.farm}.staticflickr.com/${card.server}/${card.id}_${card.secret}.jpg`">
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "Home",
	components: {},
	data() {
		return {
			cardCount: 16,
			cards: [],
			photos: [],
		};
	},
	computed: {
		gridStyles() {
			return {
				gridTemplateColumns: `repeat(${Math.sqrt(
					this.cardCount
				)}, 1fr)`,
				gridTemplateRows: `repeat(${Math.sqrt(this.cardCount)}, 1fr)`,
			};
		},
	},
	methods: {
		startGame() {
            this.cards = [];
			for (let i = 0; i < this.cardCount; i++) {
                if (this.photos[i]) {
                    this.cards.push(this.photos[i]);
                }
            }
		},
	},
	mounted() {
		this.$axios
			.get(
				`https://api.flickr.com/services/rest/?method=flickr.photos.search&api_key=11d2ecae03a542f5d205f8e0c32488f0&tags=cats&format=json&nojsoncallback=1`
			)
			.then((response) => {
				this.photos = response.data.photos.photo;
			});
	},
};
</script>

<style lang="scss" scoped>
.settings {
	height: 20rem;
	padding: 6rem 0;
	background-color: lightgray;
}

.card-container {
	display: grid;

	grid-column-gap: 4rem;
	grid-row-gap: 4rem;
	.card {
		background-color: black;
		height: 15rem;
		// width: 15rem;
		cursor: pointer;
	}
}
</style>
