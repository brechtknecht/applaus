<template>
	<div>
		<div class="meta-data-inner" v-if="!contentIsGiven">
			<router-link :to="'aussteller/'+ teaser.slug">
				<span class="authors">
					<span v-for="author in teaser.authors.split(',')">
						<span class="forename">{{ $root.getAbsolventFullname(author).forename }} </span> <span class="lastname">{{
							$root.getAbsolventFullname(author).lastname }}</span><br>
					</span>
				</span>
				<span class="title">
					{{ teaser.title }}
				</span>
			</router-link>
		</div>
		<!-- <div class="teaser" v-bind:class="{ headerHidden: this.$root.headerActive) }"> -->
		<div class="teaser">
			<div class="gradient" />
			<!-- ATTENTION — ToDo ! Insert media queries here in this img tag! (sizes) -->
			<img class="teaser-image" :src="backgroundImage[0].url" :srcset="backgroundImage[0].srcset" :sizes="$root.getImageCoverWidth(backgroundImage[0].aspect_ratio, $root.viewportWidth, $root.viewportHeight, true)"
			 v-if="this.backgroundImage.length">
		</div>
	</div>
</template>

<script>
	export default {
		props: {
			content: Array,
			contentIsGiven: Boolean
		},
		mounted() {
			// console.log(this.backgroundImage[0].url);
		},
		computed: {
			backgroundImage: function () {
				if (this.contentIsGiven) {
					return this.content
				} else {
					return this.teaser.images
				}
			},
			teaser: function () {
				return this.$root.randomProperty(this.$root.$options.pages.ausstellung.body.list)
			}
		}
	}
</script>

<style lang="scss">
	@import '@/scss/mediaqueries.scss';
		.meta-data-inner {
			position: absolute;
			z-index: 10;
			bottom: 2rem;
			left: 0;
			right: 0;
			margin: 0 auto;
			animation: fading 1s ease-in 1.8s backwards;

		@include bp(XL) {
			width: 100%;
			padding-left: 2rem;
			box-sizing: border-box;
		}

		@include bp(S) {
			display: none;
		}

		a {
			text-decoration: none;
			border: none;
		}

		.authors span,
		.title {
			color: var(--color3);
			font-family: johnston, sans-serif;
			font-size: .75rem;
			margin: 0;
			line-height: 0rem;
			font-style: normal;

			@include bp(XXL) {
				font-size: .875rem;
			}
		}

		.authors {
			margin-bottom: 0;
		}

		.authors span {
			text-transform: uppercase;
			letter-spacing: .05rem;
		}
	}
	.teaser {
		position: absolute;
		overflow: hidden;
		top: 0;
		left: 0;
		bottom: 0;
		right: 0;
		box-sizing: border-box;
		display: flex;
		flex-direction: column;
		justify-content: flex-end;
		z-index: -1;
	}

	.gradient {
		position: absolute;
		top: 0;
		left: 0;
		bottom: 0;
		right: 0;
		background: linear-gradient(var(--gradient_col2-11perc) 0, var(--gradient_col2-11perc) 1%, var(--gradient_col2-90perc) 84%, var(--color2) 100%);
		z-index: 1;
	}

	.teaser-image {
		filter: brightness(80%) saturate(60%);
		position: fixed;
		// top: 0;
		left: 0;
		// bottom: 0;
		right: 0;
		height: 110vh;
		width: 100vw;
		object-fit: cover;
		background-color: var(--color6);
		// animation: moveImage 60s cubic-bezier(.18,.01,.85,1) both infinite;
	}

	.headerHidden {
		display: none;
	}
</style>