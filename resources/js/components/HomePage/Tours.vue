<template>
    <div>
        <section class="section-style4 mb-1">
        <div class="container page-builder-ltr">
            <div class="row row-style row_a1">
                <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12 col_a1c">
                    <div class="module so-deals-1tr home1_deals so-deals">
                        <div class="head-title clearfix">
                            <div class="block-title pull-left">
                                <h3><span>{{ $t('popular') }} {{ $t('tours') }}</span></h3>
                            </div>
                            <router-link to="tours"
                               class="pull-right">
                                {{ $t('view all') }}
                                <i class="fa fa-angle-right" aria-hidden="true"></i>
                            </router-link>
                        </div>

                        <div class="modcontent">
                            <div class="popular-tours-slider">
                                <VueSlickCarousel ref="popularToursSlider" v-if="tours.length > 0" v-bind="settings">
                                    <!--  items  -->
                                    <div class="item" v-for="tour in tours" :key="tour.id">
                                        <div class="item-inner">
                                            <div class="transition product-layout">
                                                <div class="product-item-container ">
                                                    <div class="item-block so-quickview">
                                                        <div class="image">
                                                            <vue-load-image>
                                                                <img slot="image" :src="tour.image" />
                                                                <img slot="preloader" src="../../../assets/loading.gif">
                                                                <img slot="error" src="../../../assets/no_image.png">
                                                            </vue-load-image>
                                                        </div>
                                                        <div class="item-content clearfix">
                                                            <h3>{{tour.title_api}}</h3>
                                                            <div class="reviews-content">
                                                                <starRating
                                                                    :star-size="15"
                                                                    :rating="tour.star_rate ? parseInt(tour.star_rate) : 0"
                                                                    :show-rating="false"
                                                                    :read-only="true"></starRating>
                                                            </div>
                                                            <ul>
                                                                <li><i class="fa fa-map-marker" aria-hidden="true"></i> New Zealand</li>
                                                                <li><i class="fa fa-clock-o" aria-hidden="true"></i> 2 Day</li>
                                                                <li><i class="fa fa-user-circle" aria-hidden="true"></i> 4 persons</li>
                                                            </ul>
                                                            <div class="item-bot clearfix">
                                                                <div class="price text-capitalize pull-left">
                                                                <label>{{cahangePrice(tour.price_api.price)}}{{ currency ? $t(currency.abbr) : $t("LE")}}</label><span>{{ $t('person') }}</span><br>
                                                                <label>{{cahangePrice(tour.price_api.ch_price)}}{{ currency ? $t(currency.abbr) : $t("LE")}}</label><span>{{ $t('child') }}</span>
                                                                </div>
                                                                <router-link :to="'/tour'+tour.id"
                                                                    class="book-now btn-quickview quickview quickview_handler pull-right">
                                                                    {{ $t('book now') }}
                                                                 </router-link>
                                                    
                                                            </div>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </VueSlickCarousel>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    </div>
</template>
<script>
import VueSlickCarousel from 'vue-slick-carousel';
import starRating from "vue-star-rating";
import { mapGetters } from 'vuex';
export default {
    name: 'Tours',
    props: {
        tours: {type: Array, default:  () => ([])},
    },
    computed: {
        ...mapGetters(['currency']),
    },
    components: {
        VueSlickCarousel,
        starRating
    },
    data() {
        return {
            settings:{
                arrows: true,
                dots: false,
                infinite: true,
                speed: 500,
                slidesToShow: 3,
                slidesToScroll: 1,
                autoplay: true,
                autoplaySpeed: 5000,
                cssEase: "linear",
                pauseOnFocus: true,
                pauseOnHover: true,
                responsive: [
                    {
                        "breakpoint": 1024,
                        "settings": {
                            "slidesToShow": 3
                        }
                    },
                    {
                        "breakpoint": 768,
                        "settings": {
                            "slidesToShow": 2
                        }
                    },
                    {
                        "breakpoint": 480,
                        "settings": {
                            "slidesToShow": 1,
                            arrows: false,
                        }
                    }
                ]
            }
        }
    },
    methods: {
        showNext() {
            this.$refs.popularToursSlider.next()
        },
        showPrev() {
            this.$refs.popularToursSlider.prev()
        },
        cahangePrice(price){
            if(this.currency){
                return parseFloat(parseFloat(price).toFixed(2) / parseFloat(this.currency.ex_rate).toFixed(2)).toFixed(2);
            }else{
                return parseFloat(price).toFixed(2);
            }
        }
    }
}
</script>
