<template>
    <div>
        <div v-for="(song, index) in items" :key="song.id">
            <!--<div class="card" v-if="index == activeIndex">
                <div class="card__image-container">
                     <iframe :src="song.spotify" width="100%" height="80" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"></iframe>
                </div>
                
                <svg class="card__svg" viewBox="0 0 800 500">

                    <path d="M 0 100 Q 50 200 100 250 Q 250 400 350 300 C 400 250 550 150 650 300 Q 750 450 800 400 L 800 500 L 0 500" stroke="transparent" fill="#333"/>
                    <path class="card__line" d="M 0 100 Q 50 200 100 250 Q 250 400 350 300 C 400 250 550 150 650 300 Q 750 450 800 400" stroke="pink" stroke-width="3" fill="transparent"/>
                </svg>
                
                <div class="card__content">
                    <button @click="changeIndex(-1)">
                        Prev song
                    </button>
                    <button @click="changeIndex(1)">
                        Next song
                    </button>
                </div>
            </div>-->
            <div class="blog-slider" v-if="index == activeIndex">
                <div class="blog-slider__wrp swiper-wrapper">
                    <div class="blog-slider__item swiper-slide">
                        <div class="blog-slider__img">
                            <iframe :src="song.spotify" width="100%" height="80" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"></iframe>
                        </div>
                        <div class="blog-slider__content">
                            <span class="blog-slider__code"> {{song.artist.name}}</span>
                            <div class="blog-slider__title">{{ song.title }}</div>
                            <div class="blog-slider__text">  </div>
                            <div class="blog-slider__button">
                                <div @click="changeIndex(-1)"><CarouselButtonPrev ></CarouselButtonPrev></div>
                                <div @click="changeIndex(1)"><CarouselButtonNext></CarouselButtonNext></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
 import CarouselButtonPrev from "./CarouselButtonPrev.vue";
 import CarouselButtonNext from "./CarouselButtonNext.vue";
    export default {
        name: "Carousel",
        props: [
            "items",
            "activeIndex"
        ],
        components: {
            CarouselButtonPrev,
            CarouselButtonNext
        },
        methods: {
            changeIndex(value) {
                // copy, you can not change props directly
                let index = this.activeIndex;
                // -1 of 1;
                index += value;
                // check if index is the beginning or ending
                if (index < 0) {
                    index = this.items.length -1;
                } else if (index == this.items.length) {
                    index = 0;
                }
                // change index in gamepage
                this.$emit("change-index", index);
            }
        }
    }
</script>