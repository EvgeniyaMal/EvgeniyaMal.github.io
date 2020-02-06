<template>
    <div id="app">
        <SiteHeader />
        <Home v-if="goHome" />
        <Details v-bind:painterName="currentPainter" v-else />
    </div>
</template>

<script>
    import Home from './components/Home.vue';
    import SiteHeader from './components/siteHeader.vue';
    import Details from './components/details.vue';

    export default {
        name: 'app',
        data() {
            return {
                goHome: true,
                currentPainter: null
            }
        },
        components: {
            SiteHeader,
            Home,
            Details
        },
        methods: {
            swapComponent: function (goHome, painter) {
                this.goHome = goHome;
                this.currentPainter = painter;
            }
        }
    };
</script>

<style>
    article, aside, details, figcaption, figure, footer, header, hgroup, main, menu, nav, section, summary {
        display: block;
    }

    .container {
        position: relative;
        max-width: 940px;
        margin: 0 auto;
    }

    .container:after, .grid-row:after {
        content: '';
        display: table;
        clear: both;
    }

    b, strong {
        font-weight: 700;
    }
</style>

