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
						<FlipCard
							:card="card"
							@click.native="flipCard(card)"
						/>
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
	components: {
		FlipCard: () => import("@/components/FlipCard.vue"),
	},
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
		clearSelected() {
			setTimeout(() => {
				const firstRef = this.cards.find(
					(card) => card.tempId == this.firstSelected.tempId
				);
				const secondRef = this.cards.find(
					(card) => card.tempId == this.secondSelected.tempId
				);
				if (!firstRef.matched) {
					firstRef.flipped = false;
				}
				if (!secondRef.matched) {
					secondRef.flipped = false;
				}
				this.firstSelected = null;
				this.secondSElected = null;
			}, 1000);
		},
		startGame() {
			this.cards = [];
			for (let i = 0; i < this.cardCount / 2; i++) {
				if (this.photos[i]) {
					this.cards.push({
						...this.photos[i],
						tempId: (idIncrementer += 1),
						matched: false,
						flipped: false,
					});
					this.cards.push({
						...this.photos[i],
						tempId: (idIncrementer += 1),
						matched: false,
						flipped: false,
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
		flipCard(selected) {
			if (this.firstSelected === null) {
				this.firstSelected = selected;
				this.cards.find(
					(card) => card.tempId == this.firstSelected.tempId
				).flipped = true;
				console.log("first selection");
			} else {
				this.secondSelected = selected;
				this.cards.find(
					(card) => card.tempId == this.secondSelected.tempId
				).flipped = true;
				this.checkForMatch();
			}
		},
		checkForMatch() {
			if (this.firstSelected.id === this.secondSelected.id) {
				this.cards.forEach((card) => {
					if (card.id === this.firstSelected.id) {
						card.matched = true;
					}
				});
				this.clearSelected();
				console.log("matched!");
			} else {
				this.clearSelected();
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
}
</style>
