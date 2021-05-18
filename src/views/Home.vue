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
						v-for="(card) in cards"
						:key="card.tempId"
						class="card"
					>
						<img
							:src="`https://farm${card.farm}.staticflickr.com/${card.server}/${card.id}_${card.secret}.jpg`"
							@click="selectCard(card)"
						>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
let idIncrementer = 1;

export default {
	name: "Home",
	components: {},
	data() {
		return {
			//data
			cardCount: 16,
			cards: [],
			photos: [],
			//state
			firstSelected: null,
			secondSelected: null,
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
			for (let i = 0; i < this.cardCount / 2; i++) {
				if (this.photos[i]) {
					this.cards.push({
						...this.photos[i],
						tempId: (idIncrementer += 1),
					});
					this.cards.push({
						...this.photos[i],
						tempId: (idIncrementer += 1),
					});
				}
			}
			this.cards = this.shuffleCards(this.cards);
		},
		shuffleCards(array) {
			let i = array.length;
			while (i--) {
				const ri = Math.floor(Math.random() * (i + 1));
				[array[i], array[ri]] = [array[ri], array[i]];
			}
			return array;
		},
		selectCard(selected) {
			if (this.firstSelected === null) {
				this.firstSelected = selected;
				console.log("first selection");
			} else {
				this.secondSelected = selected;
				this.checkForMatch();
			}
		},
		checkForMatch() {
			if (this.firstSelected.id === this.secondSelected.id) {
				console.log("matched!");
			} else {
				this.firstSelected = null;
				this.secondSElected = null;
				console.log("no match and clear");
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
		img {
			object-fit: cover;
			height: 100%;
			width: 100%;
		}
	}
}
</style>
