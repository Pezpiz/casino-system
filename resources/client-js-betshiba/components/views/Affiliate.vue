<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col vertical-tabs-column">
                <div class="vertical-tabs">
                    <div @click="tab = 0" :class="`option ${tab === 0 ? 'active' : ''}`">
                        {{ $t('partner.tabs.overview') }}
                    </div>
                    <div v-if="!isGuest" @click="tab = 1" :class="`option ${tab === 1 ? 'active' : ''}`">
                        {{ $t('partner.tabs.list') }}
                    </div>
                    <div v-if="!isGuest" @click="tab = 2" :class="`option ${tab === 2 ? 'active' : ''}`">
                        {{ $t('partner.tabs.analytics') }}
                    </div>
                </div>
            </div>
            <div class="col vertical-tabs-content-column">
                <div class="vertical-tabs-content">
                    <div v-if="tab === 0" class="tab-content">
                        <div v-if="isGuest" v-html="$t('partner.overview.guest_content')"></div>
                        <div v-else>
                            <div v-html="$t('partner.overview.content', { id: user.user.name })"></div>
                            <div class="mt-4">
                                <div class="heading">{{ $t('partner.overview.header') }}</div>
                                <div class="subheader">{{ $t('partner.overview.1') }}</div>
                            </div>
                        </div>
                    </div>
                    <div v-if="!isGuest && tab === 1" class="tab-content" data-tab="list">
                        <table class="table dt-responsive nowrap dataTable" v-if="affiliates.affiliates && affiliates.affiliates.length > 0">
                            <thead>
                                <tr>
                                    <th>{{ $t('partner.list.name') }}</th>
                                    <th>{{ $t('partner.list.viplevel') }}</th>
                                    <th>{{ $t('partner.list.generated') }}</th>
                                </tr>
                            </thead>
                            <tbody>
                                <router-link tag="tr" :to="`/profile/${affiliate.user._id}`" v-for="affiliate in affiliates.affiliates" :key="affiliate.user._id" :style="{ cursor: 'pointer' }">
                                    <td><img alt :src="affiliate.user.avatar" style="width: 32px; height: 32px; margin-right: 5px;"> {{ affiliate.user.name }}</td>
                                    <td class="tdAffilliates">{{ affiliate.viplevel }}</td>
                                    <td class="tdAffilliates">{{ affiliate.generated }}</td>
                                </router-link>
                            </tbody>
                        </table>
                        <div v-else style="text-align: center">:(</div>
                    </div>
                    <div v-if="!isGuest && tab === 2" class="tab-content">
                        <div v-html="$t('partner.analytics.referrals_partnerbonus', { count: affiliates.partnerbonus })"></div>
                        <div v-html="$t('partner.analytics.referrals_claimable', { count: affiliates.claimable })"></div>
                        <hr>
                        <div class="btn btn-primary mt-2" @click="collectAffiliate()">CLAIM {{ affiliates.claimable }}$</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import { mapGetters } from 'vuex';
    import Bus from '../../bus';

    export default {
        data() {
            return {
                tab: 0,
                affiliates: []
            }
        },
        created() {
            axios.post('/api/promocode/affiliates').then(({ data }) => this.affiliates = data);
        },
        computed: {
            ...mapGetters(['isGuest', 'user'])
        },
        methods: {
            collectAffiliate() {
                axios.post('/api/promocode/affiliatescollect').then(() => Bus.$emit('modal:close'));
            }
        }
    }
</script>

<style lang="scss">
    @import "resources/sass/variables";

    .heading {
        font-weight: 700;
        font-size: 1.1em;
    }

    .subheader {
        margin: 10px 0;
    }

    .tdAffilliates {
border-bottom: 1px solid hsl(0deg 0% 100% / 1%) !important;
    }

    .commission-table {
        display: flex;
        flex-direction: column;
        width: fit-content;
        margin-right: auto;
        margin-left: auto;
        margin-bottom: 15px;
        margin-top: 15px;

        @include themed() {
            .commission-table-block {
                display: flex;
                background: t('input');
                padding: 0px 10px;

                &:first-child {
                    border-top-left-radius: 3px;
                    border-top-right-radius: 3px;
                    padding-top: 5px;
                }

                &:last-child {
                    border-bottom-left-radius: 3px;
                    border-bottom-right-radius: 3px;
                    padding-bottom: 5px;
                }

                div {
                    width: 38px;
                    height: 38px;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    font-size: 0.9em;
                    font-weight: 100;

                    svg {
                        width: 22px;
                        height: 22px;
                    }
                }
            }
        }
    }

    .dataTable {
        @include themed() {
            color: t('text');

            th, td {
                border-top: 1px solid rgba(t('text'), 0.1);
            }

            thead th {
                border-bottom: 2px solid rgba(t('text'), 0.17);
            }
        }
    }

    .dataTables_info {
        position: absolute;
        margin-top: -12px;
    }

    .dataTables_paginate {
        .paginate_button {
            margin: 5px;
        }
    }
</style>
