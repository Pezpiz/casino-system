<template>
    <div>
        <div :class="'mobile-menu-extended '+(expandWindow ? 'd-flex' : '')" v-if="expandWindow">
            <!--<div class="control theme-switcher" @click="$store.commit('switchTheme')">
                <i class="fas fa-moon-stars" v-if="theme === 'dark'"></i>
                <i class="fas fa-sun" v-else></i>
                <div>{{ $t('general.head.theme') }}</div>
            </div>-->
            <router-link tag="div" to="/help" :class="'control ' + ($route.path === '/help' ? 'active' : '')" @click.native="expandWindow = false">
                <i class="fas fa-question-circle"></i>
                <div>{{ $t('general.head.help') }}</div>
            </router-link>
            <router-link tag="div" to="/fairness" :class="'control ' + ($route.paht === '/fairness' ? 'active' : '')" @click.native="expandWindow = false">
                <i class="fas fa-balance-scale-right"></i>
                <div>{{ $t('fairness.title') }}</div>
            </router-link>
        </div>
        <div class="mobile-menu-games" v-if="gamesWindow" @click="gamesWindow = false">
            <div class="mobile-menu-games-container os-host-flexbox">
                <overlay-scrollbars :options="{ scrollbars: { autoHide: 'leave' }, className: 'os-theme-thin-light' }" class="games w-100">
                    <router-link to="/" class="game" @click.native="gamesWindow = false">
                        <div class="icon">
                            <i class="fas fa-spade"></i>
                        </div>
                        <div class="name">
                            {{ $t('general.head.index') }}
                        </div>
                    </router-link>

                    <router-link to="/game/category/favorite" class="game" v-if="!isGuest" @click.native="gamesWindow = false">
                        <div class="icon"><icon icon="fal fa-star"></icon></div>
                        <div class="name">{{ $t('general.sidebar.favorite') }}</div>
                    </router-link>

                    <router-link to="/game/category/recent" class="game" v-if="!isGuest" @click.native="gamesWindow = false">
                        <div class="icon"><icon icon="fal fa-history"></icon></div>
                        <div class="name">{{ $t('general.sidebar.recent') }}</div>
                    </router-link>


                <router-link to="/game/category/inhouse" class="game" @click.native="gamesWindow = false">
                    <div class="icon"><i class="fas fa-acorn"></i></div>
                    <div class="name letter-spacing">{{ $t('general.sidebar.inhouse') }}</div>
                </router-link>

                <router-link to="/game/category/slots" class="game" @click.native="gamesWindow = false">
                    <div class="icon"><i class="fak fa-777"></i></div>
                    <div class="name letter-spacing">{{ $t('general.sidebar.slots') }}</div>
                </router-link>

                <router-link to="/game/category/live" class="game" @click.native="gamesWindow = false">
                    <div class="icon"><i class="fak fa-blackjack"></i></div>
                    <div class="name letter-spacing">{{ $t('general.sidebar.live') }}</div>
                </router-link>

                <router-link to="/providers" class="game" @click.native="gamesWindow = false">
                    <div class="icon"><i class="fas fa-gamepad"></i></div>
                    <div class="name letter-spacing">{{ $t('general.sidebar.providers') }}</div>
                </router-link>  

                <router-link to="/browse" class="game" @click.native="gamesWindow = false">
                    <div class="icon"><i class="fas fa-th-large"></i></div>
                    <div class="name letter-spacing">{{ $t('general.sidebar.all') }}</div>
                </router-link> 
				
				<a onclick="window.open(window.location.origin + '/admin')" v-if="!isGuest && user.user.access === 'admin'" class="game" @click.native="gamesWindow = false">
					<div class="icon"><i class="fas fa-cog"></i></div>
					<div class="name">{{ $t('general.sidebar.admin') }}</div>
				</a>

                </overlay-scrollbars>
            </div>
        </div>
        <div class="mobile-menu">
            <div :class="'control ' + ($route.path === '/' || $route.path === '/index' ? 'active' : '')" @click="gamesWindow = !gamesWindow; expandWindow = false; $store.dispatch('toggleChat', false)">
                <i class="fas fa-spade"></i>
                <div><i :class="'fal fa-angle-up '+(gamesWindow ? 'fa-rotate-180' : '')" style="margin-right: 1px"></i> {{ $t('general.head.games') }}</div>
            </div>
            <div class="control" @click="$store.dispatch('toggleChat'); expandWindow = false; gamesWindow = false">
                <i class="fad fa-comments"></i>
                <div>{{ $t('general.head.chat') }}</div>
            </div>
            <div class="control" @click="expandWindow = false; gamesWindow = false; openFaucetModal();">
                <i class="fad fa-coins"></i>
                <div>{{ $t('general.head.bonus_short') }}</div>
            </div>
            <div :class="'control '+(expandWindow ? 'd-flex' : '')" @click="expandWindow = !expandWindow; gamesWindow = false; $store.dispatch('toggleChat', false)">
                <i :class="'fal fa-angle-up '+(expandWindow ? 'fa-rotate-180' : '')"></i>
            </div>
        </div>
    </div>
</template>

<script>
    import { mapGetters } from 'vuex';
    import AuthModal from "../modals/AuthModal";
    import FaucetModal from "../modals/FaucetModal";

    export default {
        data() {
            return {
                gamesWindow: false,
                expandWindow: false
            }
        },
        methods: {
            openLoginModal() {
                AuthModal.methods.open('auth');
            },
            openFaucetModal() {
                if(this.isGuest) return this.openLoginModal();
                FaucetModal.methods.open();
            }
        },
        computed: {
            ...mapGetters(['theme', 'games', 'isGuest', 'user'])
        }
    }
</script>
