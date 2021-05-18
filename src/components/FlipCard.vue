<template>
	<div class="card">
		<div v-if="!card.flipped" class="front face">

		</div>
		<div v-else class="back face">
			<img :src="`https://farm${card.farm}.staticflickr.com/${card.server}/${card.id}_${card.secret}.jpg`">
		</div>
	</div>
</template>

<script>
export default {
	name: "FlipCard",
	props: {
		card: {
			type: Object,
		},
	},
};
</script>

<style lang="scss" scoped>
.card {
	background-color: black;
	height: 15rem;
	cursor: pointer;

	img {
		object-fit: cover;
		height: 100%;
		width: 100%;
	}
}
</style>