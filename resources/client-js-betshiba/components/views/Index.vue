<template>
   <div class="gameCategory bg-content-gradient">
        <div class="container-fluid carousel-top">
        <div class="slider mt-0 pt-3 p-1 mb-3">
            <div class="glide" id="slider">
                <div class="glide__track" data-glide-el="track">
                    <ul class="glide__slides" style="border-radius: 18px;">
                        <li class="glide__slide" style="background: linear-gradient(to right, #8affe5, rgb(150, 255, 182)); border-radius: 18px;">
                            <img class="carousel-image" src="https://games.cdn4.dk/betshiba/banner-drop.png">
                            <div class="slideContent left">
                                <div class="slideContentWrapper">
                                    <div class="description" style="color: #382f5c; font-family: 'Proxima Soft Ex Bd;">
                                        <span style="text-transform: uppercase; letter-spacing: 0.1px;">Bonus Battle BETA is here!</span>
                                        <br>Play against friends on your favorite slots!
                                    </div>
                                    <a href="/bonusbattle" target="_blank">
                                    <div class="btn btn-carousel">
                                        Play PVP Slots
                                    </div>
                                    </a>
                                </div>
                            </div>
                        </li>  
                        <li class="glide__slide" style="background: linear-gradient(to right, #8affe5, rgb(150, 255, 182)); border-radius: 18px;">
                            <img class="carousel-image" src="https://games.cdn4.dk/betshiba/daily-drop.png">
                            <div class="slideContent left">
                                <div class="slideContentWrapper">
                                    <div class="description" style="color: #382f5c; font-family: 'Proxima Soft Ex Bd;">
                                        <span style="text-transform: uppercase; letter-spacing: 0.1px;">Daily Drops</span>
                                        <br>Extra prizes for you this winter!
                                    </div>
                                    <a href="/game/provider/pragmatic" target="_blank">
                                    <div class="btn btn-carousel">
                                        Pragmatic Play Games
                                    </div>
                                    </a>
                                </div>
                            </div>
                        </li>  


                    </ul>
                </div>
                <div class="glide__arrows" data-glide-el="controls">
                    <button class="glide__arrow glide__arrow--left" data-glide-dir="<"></button>
                    <button class="glide__arrow glide__arrow--right" data-glide-dir=">"></button>
                </div>
                <div class="glide__bullets" data-glide-el="controls[nav]">
                    <button class="glide__bullet" data-glide-dir="=0"></button>
                    <button class="glide__bullet" data-glide-dir="=1"></button>
                </div>
            </div>
        </div>
        </div>

      <template v-if="!gamesLoading">

         
        <div class="container-fluid" style="max-width: 1320px;">
                <div class="search">
					<div class="input-group mb-3">
					  <input type="text" class="form-control" v-model="keyword" autocomplete="off" :placeholder="'Search in ' + count + ' games..'">
					  <button @click="openProviders" class="btn btn-primary" type="button"><i :class="'fas ' + (ProvidersList ? 'fa-chevron-up' : 'fa-chevron-down')" aria-hidden="true"></i> Provider</button>
					</div>
			   
                  <div class="row-list">
                     <div class="list-providers" :class="{ 'show' : ProvidersList }">
                        <div class="custom-dropdown">
                            <template v-for="provider in Providers">
                               <div class="custom-control custom-checkbox">
                                    <div @click="selectProvider(provider.name)" :class="'custom-select-option ' + (checkedProviders.includes(provider.name) ? 'selected' : '')">{{ provider.name }}</div>
                               </div>
                            </template> 
                        </div>
                     </div>
                    <div class="list-categories" :class="{ 'show' : CategoriesList }">
                        <div xyz="fade appear-short-100% origin-top ease-in-out duration-7" class="custom-dropdown">
                           <div class="custom-control custom-checkbox">
                                <div @click="selectCategory('inhouse')" :class="'custom-select-option ' + (checkedCategories.includes('inhouse') ? 'selected' : '')">In-House</div>
                           </div>
                           <div class="custom-control custom-checkbox">
                                <div @click="selectCategory('slots')" :class="'custom-select-option ' + (checkedCategories.includes('slots') ? 'selected' : '')">Slots</div>
                           </div>
                           <div class="custom-control custom-checkbox">
                                <div @click="selectCategory('live-table')" :class="'custom-select-option ' + (checkedCategories.includes('live-table') ? 'selected' : '')">Live Tables</div>
                           </div>
                           <div class="custom-control custom-checkbox">
                                <div @click="selectCategory('live')" :class="'custom-select-option ' + (checkedCategories.includes('live') ? 'selected' : '')">Live</div>
                           </div>
                           <div class="custom-control custom-checkbox">
                                <div @click="selectCategory('scratch-cards')" :class="'custom-select-option ' + (checkedCategories.includes('scratch-cards') ? 'selected' : '')">Scratchcards</div>
                           </div>
                            <div class="custom-control custom-checkbox">
                                <div @click="selectCategory('multiplayer')" :class="'custom-select-option ' + (checkedCategories.includes('multiplayer') ? 'selected' : '')">Multiplayer</div>
                           </div>
                           
                           <div class="custom-control custom-checkbox">
                                <div @click="selectCategory('virtualsport');selectCategory('virtualsports')" :class="'custom-select-option ' + ((checkedCategories.includes('virtualsport') && checkedCategories.includes('virtualsports')) ? 'selected' : '')">Virtual Sports</div>
                           </div>
                           <div class="custom-control custom-checkbox">
                                <div @click="selectCategory('rollback')" :class="'custom-select-option ' + (checkedCategories.includes('rollback') ? 'selected' : '')">RollBack</div>
                           </div>
                            <div class="custom-control custom-checkbox">
                                <div @click="selectCategory('Video Slots')" :class="'custom-select-option ' + (checkedCategories.includes('Video Slots') ? 'selected' : '')">Video Slots</div>
                           </div>
                        </div>
                     </div>
                  </div>
               </div>
                     </div>
        <div class="container-fluid inner-content-max">

                <template v-if="!pageLoading">
                    <div class="warning" v-if="Object.keys(categoryGames).length === 0" v-html="$t('general.sidebar.no_search')"></div>
                 </template>
               <template v-for="(cat, key) in categoryGames">
                  <div class="games">
                     <div v-for="game in cat" :key="game.id" :class="`frame-loader game_poster_${game.type} game-${game.id} game_type-${game.type} img-hover-zoom--zoom-n-rotate`">
                        <div :class="`game_poster_${game.type}-image game_tp-image`" v-if="game.ext" @click="game.ext ? $router.push(`/casino/${game.id}`) : $router.push(`/game/${game.id}`)">
                            <img :src="`${cdnBase}${game.icon}${cdnParameters}`" alt :class="`game_poster_${game.type}-image`">
                        </div>



                        <div :class="`frame-loader game_poster_${game.type}-image game_tp-image`" v-if="!game.ext" @click="game.ext ? $router.push(`/casino/${game.id}`) : $router.push(`/game/${game.id}`)">
                            <img :src="`${cdnBase}/provablyfair/${game.id}.png${cdnParameters}`" alt :class="`game_poster_${game.type}-image`">
                           <div class="unavailable" v-if="game.d">
                              <div class="slanting">
                                 <div class="content">
                                    {{ $t(game.isPlaceholder ? 'general.coming_soon' : 'general.not_available') }}
                                 </div>
                              </div>
                           </div>
                        </div>
                        <div :class="`game_poster_${game.type}-houseEdge`" v-if="game.houseEdge" @click="game.ext ? $router.push(`/casino/${game.id}`) : $router.push(`/game/${game.id}`)">
                           {{ game.p }}
                        </div>
                        <div :class="`game_poster_${game.type}-label`" @click="game.ext ? $router.push(`/casino/${game.id}`) : $router.push(`/game/${game.id}`)">
                        <p><b class="mt-4">{{ game.name }}</b>
                        <br>
                        <small v-if="game.ext">by {{ game.p }}
                        <svg class="play-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 63 63"><defs></defs><g fill="none" fill-rule="evenodd"><circle cx="31.3" cy="31.3" r="30.3" fill="#128cfe" fill-opacity=".2" stroke="#E5E7EE" stroke-width="2"></circle><path fill="#F5F6F7" d="M39.5 34.3l-11.3 7.5a2 2 0 01-3-1.6v-15a2 2 0 013-1.7L39.5 31a2 2 0 010 3.3z"></path></g></svg>
                        </small>
                        <small v-if="!game.ext">in-house provably fair</small> 

                        <small :style="`opacity: 0.9; font-size: 10px;`">{{ game.houseEdge }}% Edge</small> 

                         </p>
                        </div>

                        <div :class="`game_poster_${game.type}-footer`" @click="toggleFavoriteGame(game.id)" v-if="!isGuest">
                           <template v-if="!favMarkLoading">
                              <i :class="`fa${!user.user.favoriteGames || !user.user.favoriteGames.includes(game.id) ? 'l' : 's'} fa-star`"></i>
                              {{ $t(!user.user.favoriteGames || !user.user.favoriteGames.includes(game.id) ? 'general.sidebar.mark_as_favorite' : 'general.sidebar.remove_from_favorite') }}
                           </template>
                           <loader v-else></loader>
                        </div>

                     </div>

                  </div>
               </template>
            </div>

         <template v-if="!pageLoading">
            <template v-if="(page * depth < count)">
               <div class="divider">
                  <div class="line"></div>
                  <div class="divider-title">
                     <div class="show-more_progress-track">
                        <div :style="{ width: ((page * depth / count) * 100) + '%' }" class="show-more_progress-bar"></div>
                     </div>
                     <div class="show-more_text">Shown <span> {{ page * depth }} </span> from <span>{{ count }}</span> games</div>
                     <button @click="pageLoad()" href="javascript:void(0)" class="btn show-more btn-primary"><i class="fas fa-random" aria-hidden="true"></i> Show more</button>                
                  </div>
                  <div class="line"></div>
               </div>
            </template>
         </template>
         <div v-else class="games-load">
            <loader></loader>
         </div>
      </template>
      <div v-else class="games-load">
         <loader></loader>
      </div>
   </div>
</template>

<script>
    import Bus from '../../bus';
    import Glide from '@glidejs/glide';
    import { mapGetters } from 'vuex';
    import FaucetModal from "../modals/FaucetModal";
    import AuthModal from "../modals/AuthModal";
    import PasswordResetModal from "../modals/PasswordResetModal";


    export default {
        data() {
            return {
                categoryGames: {},
                gamesData: [],
                gamesLoading: true,
                pageLoading: true,
                page: 0,
                depth: 36,
                count: 0,
                favMarkLoading: false,
                ProvidersList: false,
                CategoriesList: false,
                checkedProviders: [],
                checkedCategories: [],
                Providers: [],
                keyword: null,
                timer: 0
            }
        },
        computed: {
            ...mapGetters(['cdnBase', 'cdnParameters', 'user', 'isGuest'])
        },
        watch: {
            gamesData() {
                this.categoryGames = {};
                this.load();
            },
            keyword(after, before) {
                if (this.timer) {
                    clearTimeout(this.timer);
                    this.timer = null;
                }
                this.timer = setTimeout(() => { 
                    this.gamesData = [];
                    this.categoryGames = {};
                    this.page = 0;
                    this.pageLoad();
                }, 800);
            },
            checkedProviders() {
                this.gamesData = [];
                this.categoryGames = {};
                this.page = 0;
                this.pageLoad();
            },
            checkedCategories() {
                this.gamesData = [];
                this.categoryGames = {};
                this.page = 0;
                this.pageLoad();
            }
        },
        created() {

            axios.post('/api/data/providers').then(({ data }) => {
                this.Providers = data[0].providers;
            });
            var payload = {
                page: this.page
            };
            axios.post('/api/data/games', payload).then(({ data }) => {
                this.page += 1;
                this.gamesData = data[0].games;
                this.count = data[0].count;
                if(this.gamesData.length === 0) {
                    this.$router.push('/404');
                    return;
                }
                this.gamesLoading = false;
                this.pageLoading = false;
            });
            this.load();
        },
        methods: {
            openFaucetModal() {
                FaucetModal.methods.open();
            },
            openAuthModal(type) {
                AuthModal.methods.open(type);
            },
            load() {            
                let validateUrlCategory = true;
                let games = this.gamesData;
                let duplicates = [];
                _.forEach(games, (game) => {
                    if (duplicates.includes(game.id)) return;
                    duplicates.push(game.id);

                    if (!this.categoryGames['all']) this.categoryGames['all'] = [game];
                    else this.categoryGames['all'].push(game);
                });
            }, 
            pageLoad() {
                this.pageLoading = true;
                this.categoryGames = {};
                var payload = {
                    page: this.page,
                    text: this.keyword,
                    category: this.checkedCategories,
                    provider: this.checkedProviders
                };
                axios.post('/api/data/games', payload).then(({ data }) => {
                    this.page += 1;
                    this.gamesData = this.gamesData.concat(data[0].games);
                    this.count = data[0].count;
                    this.pageLoading = false;
                });
                this.load();
                if(this.$route.params.user && this.$route.params.token) PasswordResetModal.methods.open(this.$route.params.user, this.$route.params.token);
            },
            toggleFavoriteGame(id) {
                if(this.favMarkLoading) return;
                this.favMarkLoading = true;
                axios.post('/api/user/markGameAsFavorite', { id: id }).then(() => {
                    this.$store.dispatch('update');
                    this.favMarkLoading = false;
                }).catch(() => this.favMarkLoading = false);
            },
            initSlider() {
                try {
                    if(window.$indexGlide) window.$indexGlide.destroy();
                    const glide = new Glide('#slider', {
                        type: 'slider',
                        perView: 1,
                        focusAt: 'center',
                        gap: 0,
                        autoplay: 8000,
                        keyboard: false
                    });
                    glide.mount();
                    window.$indexGlide = glide;
                } catch (ignored) {}
            },
            openProviders() {
                this.CategoriesList ? (this.CategoriesList = false) : (this.CategoriesList = false);
                this.ProvidersList = !this.ProvidersList;
            },
            openCategories() {
                this.ProvidersList ? (this.ProvidersList = false) : (this.ProvidersList = false);
                this.CategoriesList = !this.CategoriesList;
            },
            selectProvider(item) {
                if(this.checkedProviders.indexOf(item) != -1 ) {
                    this.checkedProviders.splice((this.checkedProviders.indexOf(item)), 1);
                }  else {
                    this.checkedProviders.push(item);
                }
            },

            selectCategory(item) {
                if(this.checkedCategories.indexOf(item) != -1 ) {
                    this.checkedCategories.splice((this.checkedCategories.indexOf(item)), 1);
                }  else {
                    this.checkedCategories.push(item);
                }
            }
        },
        mounted() {
            this.initSlider();

            Bus.$on('layoutSizeChange', () => setTimeout(this.initSlider, 200), true);

            const resize = () => {
                this.initSlider();
                setTimeout(this.initSlider, 301);
            };

            resize();

            $(window).on('resize', this.initSlider);
            $(window).on('load', resize);
            $(document).ready(resize);
        }
    }
</script>

<style lang="scss" scoped>
    @import "resources/sass/variables";

    @import "~@glidejs/glide/dist/css/glide.core.min.css";
    @import "~@glidejs/glide/dist/css/glide.theme.min.css";

 
        .carousel-top {
            max-width: $carousel-max-width;
        }

    .slider {
        min-height: 270px;
        height: 22vmax;
        max-height: 340px;
        display: flex;
        margin-bottom: 15px;
        margin-top: -16px;
        //box-shadow: #0003 0 4px 6px -1px, #0000001f 0 2px 4px -1px;
        width: calc(100%) !important;

        #slider {
            width: 0;
            flex: 1;
        }

        .glide {
            height: 100%;

            .glide__track {
                height: 100%;
            }

            .glide__slides {
                height: 100%;
            }

            .glide__arrow--left, .glide__arrow--right {
                padding: 0 !important;
                border: none !important;
                border-radius: 0 !important;
                box-shadow: none !important;
                height: 35px;
                width: 15px;
                opacity: 0.5 !important;
                transition: opacity 0.3s ease;
                &:hover {
                    opacity: 1 !important;
                }
            }

            .glide__arrow--right {
                background: url("data:image/svg+xml;charset=utf-8,%3Csvg width='14' height='37' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M13.738 18.663l-12.448 18L0 34.8l11.176-16.136L0 2.527 1.29.663l12.448 18z' fill='%23fff'/%3E%3C/svg%3E");
            }

            .glide__arrow--left {
                background: url("data:image/svg+xml;charset=utf-8,%3Csvg width='16' height='36' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M.994 18l12.928 18 1.34-1.864L3.655 18 15.262 1.864 13.922 0 .994 18z' fill='%23fff'/%3E%3C/svg%3E");
            }

            .glide__bullet--active {

                @include themed() {
                    background-color: rgba(t('secondary'), .95);
                    
                }
            }

            .glide__slide {
                width: 100%;
                height: 100% !important;
                position: relative;
                background-size: cover !important;

                @include themed() {
                    //background-color: rgba(t('sidebar'), .8);
                    backdrop-filter: blur(20px);
                    //border-bottom: 2px solid t('border');
                }

                background-position: center;
                background-repeat: no-repeat;

                .image {
                    background-size: cover;
                    background-repeat: no-repeat;
                    height: 90px;
                    width: 190px;
                    z-index: 1;
                    filter: drop-shadow(1px 1px 2px rgba(0, 0, 0, .2));
                }

                .carousel-image {
                    position: absolute;
                    left: 0;
                    top: 0;
                    width: 100%;
                    height: 100%;
                    -o-object-fit: contain;
                    object-fit: contain;
                    -o-object-position: top right;
                    object-position: top right;
                    pointer-events: none;
                    -webkit-user-drag: none;
                    -webkit-user-select: none;
                    -moz-user-select: none;
                    -ms-user-select: none;
                    user-select: none;
                    z-index: -1;
                }

                .slideContent {
                    width: 70%;
                    display: flex;
                    margin-left: auto;
                    margin-right: auto;
                    height: 100%;
                    flex-direction: column;
                    color: white;
                    position: relative;
                    z-index: 2;

                    &.right {
                        align-items: flex-end;
                        text-align: right;

                        .slideContentWrapper {
                            align-items: flex-end;
                        }
                    }
                    &.left {
                        align-items: flex-start;
                        text-align: left;

                        .slideContentWrapper {
                            align-items: flex-start;
                        }
                    }

                    .slideContentWrapper {
                        display: flex;
                        flex-direction: column;
                        margin-top: auto;
                        margin-bottom: auto;
                    }

                    .header, .description {
                        text-shadow: 1px 1px 3px rgba(black, 0.1);
                    }

                    .header {
                        font-weight: 600;
                        font-size: 2em;
                        margin-bottom: 25px;
                    }

                    .description {
                        font-size: $carousel-font-size;
                        font-family: $carousel-font-family;
                        line-height: $carousel-font-lineheight;
                        min-width: $carousel-description-min-width;
                        margin-bottom: 15px;
                        width: $carousel-description-width;
                        max-width: $carousel-description-width * 2;

                    }



                    .button {
                        width: 150px;
                        text-align: center;
                        padding: 10px 25px;
                        background: rgba(white, 0.1);
                        border: 1px solid rgba(white, 0.15);
                        cursor: pointer;
                        transition: background 0.3s ease, border-color 0.3s ease;

                        &:hover {
                            background: rgba(white, 0.2);
                            border-color: rgba(white, 0.25);
                        }
                    }
                }
            }
        }
    }

    @include media-breakpoint-up(lg) {
        .slider {
            width: 100%;
        }
    }

    @include media-breakpoint-down(md) {
        .slider {
            margin-left: -15px;
            width: calc(100% + 20px) !important;
            height: 200px !important;
            min-height: 200px !important;


            .glide__slides, .glide__slide {
                border-radius: 0px !important;
            }


            .image {
                height: 45px !important;
                width: 70px !important;
            }

            .button {
                width: 100px !important;
                font-size: 0.8em !important;
                padding: 5px !important;
            }

            .glide__arrow--left {
                left: 2em !important;
            }

            .glide__slide {
                background-size: contain !important;
            }

            .glide__slide {
                border-radius: 0;
            }

            .slideContentWrapper {
                .header {
                    font-size: 1.6em !important;
                }

                .description {
                    font-size: 1em !important;
                }
            }
        }
    }

    @include media-breakpoint-down(sm) {
        .image {
            width: 100%;
            background-position: center;
        }

        .slideContent .description {
            width: calc(100% - 95px) !important;
            margin-bottom: 15px !important;
        }

        .slideContent .header {
            margin-bottom: 15px !important;
        }

        .glide__bullets, .glide__arrows {
            display: none;
        }
    }
        .search {
        width: auto;
        margin-bottom: auto;
        margin-top: 20px;
        min-height: 60px;
        padding: 10px;
        justify-content: center;
        border-radius: 16px;
		
		.form-control:focus {
			@include themed() {
                color: t('text');
				background: t('header');
				border-color: t('secondary');
            }
			
			outline: 0;
			box-shadow: none;
		}
        
        .custom-select-option {
            display: flex;
            align-items: center;
            font-size: medium;
            position: relative;
            cursor: pointer;
            text-transform: capitalize;
        }

        .custom-select-option::before {
            content: '';
            display: flex;
            justify-content: center;
            align-items: center;
            width: 1.2rem;
            height: 1.2rem;
            border-radius: .25rem;
            @include themed() {
                border: 1px solid darken(t('secondary'), 20%);
            }
            margin-right: .5rem;
            flex-shrink: 0;
        }

        .custom-select-option.selected::before {
            @include themed() {
                border-color: t('secondary');
            }
        }

        .custom-select-option.selected::after {
            content: '';
            display: block;
            width: .7rem;
            height: .7rem;
            border-radius: .15rem;
            @include themed() {
                background-color: t('secondary');
            }
            position: absolute;
            left: calc(.25rem)
        }
                    
        .row-list {
            display: flex;
            flex-wrap: wrap;
            
            .col-md-10,.col-md-2 {
                position: relative;
                width: 100%;
                padding-right: 5px;
                padding-left: 5px;
            }
            
            @media (min-width: 768px) {
                .col-md-10 {
                    flex: 0 0 80%;
                    max-width: 80%;
                }
                
                .col-md-2 {
                    flex: 0 0 20%;
                    max-width: 20%;
                }
            }
            
        }
        
        @media (max-width: 768px) {
            .row-list {
                margin-right: -15px;
                margin-left: -15px;
            }
        }
        
        .search-input {
            width: 100%;
            caret-color: #ffffff;
            font-size: 14px;
            font-weight: 300;
            border-radius: 16px;
            color: #fff;
            transition: width 0.4s linear;
            border-radius: .15rem;
            @include themed() {
                background: t('header');
            }
            margin-top: 5px;
            height: 100%;
        }
        
        .provider-select-menu,.categories-select-menu {
            margin: 10px 10px 0px 10px;
            
            .searchbar {
                padding: 12px 10px;
            }
            
        }
        
        .list-providers {
            width: 100%;
            margin-bottom: 20px;
            display: none;
            
            .custom-dropdown {
                display: grid;
                grid-template-columns: repeat(auto-fit, minmax(8rem, 9rem));
                position: relative;
                top: 20px;
				@include themed() {
					background: rgba(t('sidebar'), .8);
				}
                opacity: 1;
                gap: 0.6rem 1rem;
                padding: 1.2rem;
                border-radius: 0.625rem;
                transition: opacity 150ms ease 0s;
                justify-content: space-evenly;
            }
            
        }
        
        .list-categories {
            width: 100%;
            margin-bottom: 20px;
            display: none;
            
            .custom-dropdown {
                display: flex;
                position: relative;
                top: 20px;
				@include themed() {
					background: rgba(t('sidebar'), .8);
				}
                opacity: 1;
                gap: 0.6rem 1rem;
                padding: 1.2rem;
                border-radius: 0.625rem;
                transition: opacity 150ms ease 0s;
                flex-direction: row;
                flex-wrap: wrap;
                justify-content: space-evenly;
            }
            
        }
        
        .custom-dropdown .custom-control {
            position: relative;
            z-index: 1;
            display: block;
            min-height: 1.08rem;
            -webkit-print-color-adjust: exact;
            user-select: none;
        }
        
        .list-providers.show,.list-categories.show {
            display: block;
        }
        
    }
    
    .games-load {
        display: flex;
        align-items: center;
        justify-content: center;
        margin-top: 30px;
    }
    
    .show-more_progress-track {
        @include themed() {
            background-color: darken(t('secondary'), 20%);
        }
    }
    
    .show-more_progress-bar {
        height: 0.5rem;
        border-radius: 0.5rem;
        @include themed() {
            background-color: t('secondary');
        }
    }
    
    .show-more_text {
		user-select: none;
        font-size: 0.7rem;
        margin-bottom: 10px;
    }
    
    .show-more_progress-track {
        width: 10rem;
        max-width: 100%;
        height: 0.5rem;
        border-radius: 0.5rem;
        margin-top: 20px;
        margin-bottom: 15px;
    }
    
    .btn.show-more {
        padding: 8px 20px;
    }

    .warning {
        width: 100%;
        text-align: center;
        font-size: 1.1em;
        margin-top: 15px;
        margin-bottom: 15px;
     }

    .gameCategory {
            @include themed() {
    
            .header {
                background: rgba(t('sidebar'), .8);
                backdrop-filter: blur(20px);
                border-bottom: 2px solid t('border');
                margin-top: -15px;
                padding: 30px 35px;
                font-size: 1.5em;
                position: static;
                top: 73px;
                z-index: 555;
            }
        }
    }
    

    .index_cat {
        font-size: 1.15em;
        font-weight: 600;
        margin-bottom: 15px;
        margin-top: 25px;
        padding-left: 40px;
        display: flex;
        flex-direction: row;
        align-items: center;

        i {
            margin-right: 5px;
            width: 20px;
            text-align: center;
        }
    }


@keyframes loading {  
  to {
    background-position: 315px 0, 0 0, 0 190px, 50px 195px;
  }
}

    .games {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }


    @include media-breakpoint-down(md) {    
        .slider .glide .glide__slide .carousel-image {
             opacity: 0.25;
         }

        .slideContent .description {
            width: calc(100% - 15px) !important;
            margin-bottom: 15px !important;
        }

        .slideContent .header {
            margin-bottom: 15px !important;
        }

        .glide__bullets, .glide__arrows {
            display: none;
        }
    }

    @media(max-width: 450px) {
        .index_cat {
            padding-left: 0 !important;
        }
    }
</style>
