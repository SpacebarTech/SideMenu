<template lang='pug'>
  .side-menu.swiper-container(ref='swiper' :class='[state, side]')
    .swiper-wrapper
      .swiper-slide(:class='{ empty : side === "right" }')
        .click-outside-catcher(v-if='side === "left"' @click='close')
        .main(v-if='side === "left"')
          slot(v-if='side === "left"' name='content')
      .swiper-slide(:class='{ empty : side === "left" }')
        .click-outside-catcher(v-if='side === "right"' @click='close')
        .main(v-if='side === "right"')
          slot(v-if='side === "right"' name='content')
</template>

<script>
	import Swiper from 'swiper';
	import On from '@spacebartech/on';
  import 'swiper/dist/css/swiper.css';

	export default {
		props : {
			side : {
				type    : String,
				default : 'left',
			},
			state : {
				required  : true,
				type      : String,
				validator : value => ['open', 'closed'].indexOf( value ) !== -1,
			}
		},

		computed : {
			startingSlide() {
				return this.side ==='left' ? 0 : 1;
			},

			closingSlide() {
				return ( this.startingSlide + 1 ) % 2;
			}
		},

		data : () => ( {
			swiper      : null,
			activeIndex : 0,
		} ),

		/* life cycle */

		mounted() {
			this.initSwiper();

			On.resize( () => this.initSwiper() );
		},

		/* methods */

		methods : {

			initSwiper() {

				if ( this.swiper ) {
					this.swiper.destroy();
				}

				const initialSlide = this.side === 'left' ? 1 : 0;
				const self         = this;

				this.swiper = new Swiper( this.$refs.swiper, {
					// auto initialize this swiper
					init : true,
					initialSlide,

					// swiper settings
					slidesPerView   : 1,   // force every slide to be 100vw
					speed           : 400, // ms for when you let go of swipe
					spaceBetween    : 0,   // no space between
					resistanceRatio : 0,   // stops you from swiping past all slides
					direction       : 'horizontal',

					threshold : 50, // min distance for scroll to work

					on : {

						slideChangeTransitionEnd() {
							self.activeIndex = this.activeIndex;
						}

					}
				} );
			},

			close() {
				if ( !this.swiper ) {
					return;
				}

				this.swiper.slideTo( this.closingSlide );
			}

		},

		watch : {

			state( state ) {
				const { swiper, startingSlide, closingSlide } = this;

				if ( state === 'closed' ) {
					swiper.slideTo( closingSlide );
				}

				else if ( state === 'open' ) {
					swiper.slideTo( startingSlide );
				}
			},

			activeIndex( i ) {

				if ( i === this.closingSlide && this.state !== 'closed' ) {
					this.$emit( 'close' );
				}

			}

		}
	}
</script>

<style lang='scss'>
$shadowRight : 2px 0px 24px rgba(0,0,0,0.08), 2px 0px 4px rgba(0,0,0,0.06);
$shadowLeft  : -2px 0px 24px rgba(0,0,0,0.08), -2px 0px 4px rgba(0,0,0,0.06);

.side-menu {
	pointer-events: none;
	width: 330px;
	height: 100vh;
	position: absolute;
	top: 0;
	overflow: visible;

	&.closed {

		.click-outside-catcher {
			pointer-events: none;
		}

		.main {
			box-shadow: none !important;
		}
	}

	&.left {
		left: 0;

		.click-outside-catcher {
			left: 0;
		}

		.main {
			box-shadow: $shadowRight;
		}
	}

	&.right {
		right: 0;

		.click-outside-catcher {
			right: 0;
		}

		.main {
			box-shadow: $shadowLeft;
		}
	}

	.swiper-slide {

		&:not(.empty) {
			pointer-events: all;

			.click-outside-catcher {
				opacity: 0;
				height: 100vh;
				width: 100vw;
				position: absolute;
				top: 0;
				z-index: 1;
			}

			.main {
				height: 100%;
				z-index: 2;
				position: relative;
				background: white;
				transition: box-shadow 0.2s ease;
			}
		}
	}
}

</style>
