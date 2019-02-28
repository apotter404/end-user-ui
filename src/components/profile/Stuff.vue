<template>
    <b-container fluid>
        <template v-if="requestsLoaded">
            <b-tabs class='mt-4' v-if="ipstuff.length > 0" @click="testForReload">
                <b-tab title='IPStuff' active>
                    <fr-ip-stuff :ipstuff="ipstuff"></fr-ip-stuff>
                </b-tab>
            </b-tabs>
            <div v-else>
                <fr-center-card :showLogo="false" class="mt-5">
                    <b-card-body slot="center-card-body">
                        <img src="static/images/empty-box.svg" class="mb-4" :alt="$t('common.form.logo')" style="width:150px;"/>
                        <h5 class="h5">{{'Unable to retrieve IPStuff'}}</h5>
                    </b-card-body>
                </fr-center-card>
            </div>
        </template>
    </b-container>
</template>

<script>
    // import _ from 'lodash';
    import CenterCard from '@/components/utils/CenterCard';
    import IPStuff from '@/components/stuff/IPStuff';
    // import axios from 'axios';

    /**
     * @description Controlling component for sharing resources, this UI is primarily focused making use of AM and its UMA features.
     * This UI feature requires full stack (IDM/AM) to be configured and for AM to be properly configured to make use of UMA
     **/
    export default {
        name: 'Stuff',
        components: {
            'fr-ip-stuff': IPStuff,
            'fr-center-card': CenterCard
        },
        data () {
            return {
                requestsLoaded: false,
                ipstuff: '',
                delayedUpdate: false
            };
        },
        beforeMount () {
            this.loadData();
        },
        methods: {
            loadData () {
                /* istanbul ignore next */
                let url = 'http://ip-api.com/json/',
                    stuffService = this.getRequestService({url: url, baseURL: url, headers: {'content-type': 'application/json'}}),
                    _this = this;

                /* istanbul ignore next */
                stuffService.get(url).then(function (response) {
                    _this.setData(response);
                })
                .catch((error) => {
                    /* istanbul ignore next */
                    if (error.response) {
                        let errorMsg = error.response.data.message;
                        this.displayNotification('error', errorMsg);
                    }
                });
            },
            setData (response) {
                this.ipstuff = JSON.stringify(response.data);
                this.requestsLoaded = true;
            },
            testForReload () {
                if (this.delayedUpdate === true) {
                    this.delayedUpdate = false;

                    this.loadData();
                }
            }
        }
    };
</script>

<style lang='scss' scoped>
</style>
