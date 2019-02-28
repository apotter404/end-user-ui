<template>
    <div>
    <span>{{ipstuff}}</span>
    </div>
</template>

<script>
    // import _ from 'lodash';

    /**
     * @description Main component for UMA (AM/IDM) displays a list of resource activities
     *
     **/
    export default {
        name: 'IPStuff',
        props: ['ipstuff'],
        data () {
            return {};
        }
    };
</script>
