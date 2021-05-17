<template>
	<div class="home">
		<div class="settings">
			<div class="outer-container">
				<div class="input-group">
                    <label for="cardCount">How many cards?</label>
                    <input type="text" id="cardCount" v-model="cardCount">
                </div>
				<div class="input-group">
                    <label for="theme">What kind of pictures?</label>
                    <input type="text" id="theme">
                </div>
			</div>
		</div>

        <div class="card-grid">
            <div class="outer-container">
                <div class="card-container" :style="gridStyles">
                    <div v-for="index in Number(cardCount)" :key="index" class="card">
                        card
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
        };
    },
    computed: {
        gridStyles() {
            return {
                gridTemplateColumns: `repeat(${Math.sqrt(this.cardCount)}, 1fr)`,
                gridTemplateRows: `repeat(${Math.sqrt(this.cardCount)}, 1fr)`,
            }
        }
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
