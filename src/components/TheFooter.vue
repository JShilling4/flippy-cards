<template>
	<footer class="footer">
		<div class="outer-container">
			<div class="logo-container">
				<img
					src="@/assets/images/logo.svg"
					alt="Blogr logo"
				>
			</div>

			<div class="footer-nav">
				<ul>
					<li>Product</li>
					<li>Overview</li>
					<li>Pricing</li>
					<li>Marketplace</li>
					<li>Features</li>
					<li>Integrations</li>
				</ul>

				<ul>
					<li>Company</li>
					<li>About</li>
					<li>Team</li>
					<li>Blog</li>
					<li>Careers</li>
				</ul>

				<ul>
					<li>Connect</li>
					<li>Contact</li>
					<li>Newsletter</li>
					<li>LinkedIn</li>
				</ul>
			</div>
		</div>
	</footer>
</template>

<script>
export default {
	name: "TheFooter",
};
</script>

<style lang="scss" scoped>
.footer {
	margin-top: 10rem;
	padding: 10rem 0;
	background-color: black;
	// border-top-right-radius: 12rem;
	border-top: 5px solid white;
	@include breakpoint(laptop) {
		margin-top: 0;
	}
	@include breakpoint(tablet-land) {
		padding: 8rem 0;
	}
	@include breakpoint(mobile) {
		padding: 8rem 0 4rem;
	}
	@include breakpoint(mobile-small) {
		padding: 8rem 0 0;
	}
	.outer-container {
		display: flex;
		@include breakpoint(tablet-land) {
			flex-wrap: wrap;
			justify-content: center;
		}
	}

	.logo-container {
		width: 30%;
		@include breakpoint(tablet-land) {
			width: 100%;
			text-align: center;
			margin-bottom: 6rem;
		}
		img {
			width: 12rem;
		}
	}

	.footer-nav {
		display: flex;
		justify-content: space-between;
		width: 55%;
		color: white;
		@include breakpoint(tablet-land) {
			width: 80%;
		}
		@include breakpoint(tablet-port) {
			width: 100%;
		}
		@include breakpoint(mobile) {
			flex-wrap: wrap;
		}
		ul {
			display: flex;
			flex-direction: column;
			align-items: flex-start;
			@include breakpoint(mobile) {
				width: 100%;
				text-align: center;
				margin-bottom: 6rem;
			}
		}
		li {
			position: relative;
			font-size: 2rem;
			@include breakpoint(mobile) {
				font-size: 1.8rem;
			}
		}
		li:first-of-type {
			font-weight: 600;
			margin-bottom: 3rem;
		}
		li:not(:first-of-type) {
			color: darken(white, 15%);
			padding: 0.75rem 0;
			cursor: pointer;
			&:after {
				position: absolute;
				bottom: 2px;
				left: 50%;
				transform: translateX(-50%);
				width: 0;
				content: "";
				height: 1px;
				background-color: transparent;
				transition: width 0.3s, background-color 0.3s;
			}
			&:hover {
				&:after {
					width: 100%;
					background-color: white;
				}
			}
		}
	}
}
</style>