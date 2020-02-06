<template>
    <div class="home">
        <section class="search-panel">
            <section class="search-panel__top">
                <div class="container">
                    <find-painter-input>
                        <p class="search-panel__label search-panel__label--wide">HAE</p>
                        <form class="search-panel__input-wrap">
                            <input class="search-panel__input" v-model="nameFilter" type="text" placeholder="Search by Name">
                            <span class="search-panel__search">
                                <i class="fa fa-search fa-lg"></i>
                            </span>
                        </form>
                    </find-painter-input>
                    <find-painter-sort >
                        <nav class="search-panel__sort">
                            <p class="search-panel__label">Lajitteluperuste</p>
                            <ul class="search-panel__options panel-options">
                                <li v-for="(option, idx) in sortOptions" :key="idx" class="panel-options__item">
                                    <a href="#" v-on:click="changeSortOption(option)" v-bind:class="[option === selectedSortOption ? 'panel-options__link--active' : '']" class="panel-options__link">{{option}}</a>
                                </li>
                            </ul>
                        </nav>
                    </find-painter-sort>
                </div>
            </section>
            <section class="search-panel__bottom">
                <div class="container">
                    <div class="search-panel__label">Valitse maalausty&#246;</div>
                    <ul class="panel-tags">
                        <li v-for="(certFilter, idx) in certificateFiltersList" :key="idx" class="panel-tags__item">
                            <div class="button button-neon" v-on:click="filterPaintersByCert(certFilter)" v-bind:class="activeButton(certFilter)" id="button-anim">
                                <div id="translate"></div>
                                <a href="#">{{certFilter}}</a>
                            </div>
                        </li>
                    </ul>
                </div>
                <div class="search-panel__description">
                    <p class="container">Etsi projektiisi sopivia urakoitsijoita ja pyyd&#228; tarjous!</p>
                </div>
            </section>
        </section>
        
        <div style="position:relative;">
            <section class="container">
                <div v-for="painter in filteredPaintersList" :key="painter.id" class="painter-short">
                    <div class="painter-short__logo">
                        <img :src="getPainterImg(painter.logo)" :alt="painter.name">
                    </div>
                    <article class="painter-short__info">
                        <h2 class="painter-short__name"><a href="#" v-on:click="swapComponent(painter.name)">{{painter.name}}</a></h2>
                        <p class="painter-short__location">
                            <img src="https://img.icons8.com/dusk/50/000000/order-delivered.png" class="painter-short__logo-img">
                            {{painter.location}}
                        </p>
                        <p class="painter-short__phone">
                            <img src="https://img.icons8.com/dusk/50/000000/phone.png" class="painter-short__logo-img">
                            <a :href="'tel:${painter.phone}'">{{painter.phone}}</a>
                        </p>
                        <p class="painter-short__mail">
                            <img src="https://img.icons8.com/dusk/50/000000/gmail.png" class="painter-short__logo-img">
                            <a :href="'mailto:${painter.mail}'">{{painter.mail}}</a>
                        </p>
                        <p class="painter-short__description">{{painter.description}}</p>
                        <ul class="tag-list">
                            <li v-for="(certificate, idx) in painter.certificates" :key="idx" class="tag-item">
                                <span class="certificate">{{certificate}}</span>
                            </li>
                        </ul>
                    </article>
                    <div class="painter-short__actions">
                        <ul class="painter-short painter-icons\">
                            <li title="Projektien määrä" class="painter-icons__item">
                                <img src="https://img.icons8.com/dusk/50/000000/real-estate.png">
                                <strong class="painter-icons__text">{{painter.workSamples}}</strong>
                            </li>
                        </ul>
                    </div>
                </div>
            </section>
        </div>
    </div>
</template>

<script>
    import jsonData from '../json/data.json'
    export default {
        name: 'Home',
        data() {
            return {
                paintersList: jsonData,
                selectedCertFilters: [],
                nameFilter: "",
                sortOptions: ["Työnäytteet", "Nimi", "Palaute"],
                selectedSortOption: ""
            }
        },
        computed: {
            certificateFiltersList() {
                var filters = [];
                for (var painter in this.paintersList) {
                    for (var cert in this.paintersList[painter].certificates) {
                        if(filters.indexOf(this.paintersList[painter].certificates[cert]) < 0)
                            filters.push(this.paintersList[painter].certificates[cert]);
                    }
                }
                return filters;
            },
            filteredPaintersList() {
                if (this.selectedCertFilters.length == 0 & this.nameFilter == "")
                    return this.paintersList;

                return this.paintersList.filter(painter => {
                    var certFilterPassed = true;
                    this.selectedCertFilters.forEach(filter => {
                        certFilterPassed &= painter.certificates.includes(filter);
                    });
                    return painter.name.toLowerCase().includes(this.nameFilter.toLowerCase()) && certFilterPassed;
                });
            }
        },
        methods: {
            filterPaintersByCert: function (cert) {
                var idx = this.selectedCertFilters.indexOf(cert);
                if (idx == -1)
                    this.selectedCertFilters.push(cert);
                else
                    this.selectedCertFilters.splice(idx, 1);
            },
            activeButton: function (cert) {
                if (this.selectedCertFilters.indexOf(cert) > -1)
                    return "button-anim-active";
                else
                    return "";
            },
            changeSortOption: function (option) {
                this.selectedSortOption = option;
                if (option === "Nimi")
                    this.paintersList.sort((a, b) => (a.name > b.name) ? 1 : -1);
                if (option === "Työnäytteet")
                    this.paintersList.sort((a, b) => (a.workSamples > b.workSamples) ? 1 : -1);
                if (option === "Palaute")
                    this.paintersList.sort((a, b) => (a.feedback.length > b.feedback.length) ? 1 : -1);
            },
            getPainterImg: function (source) {
                if (source == "")
                    return "https://img.icons8.com/dusk/50/000000/cancel-2.png";
                else
                    return source;
            },
            swapComponent: function (painterName) {
                this.$parent.swapComponent(false, painterName);
            }
        }
    }
</script>

<style scoped>
    .home {
        padding-bottom: 40px;
    }

    p:last-of-type {
        margin-bottom: 0;
    }

    .search-panel{
        color: rgb(79, 79, 79);
    }
    .search-panel__top {
        padding: 12px 0;
        background: linear-gradient(0deg,#e2e2e1,#fffffe);
    }

    .search-panel__input-wrap {
        display: inline-block;
        position: relative;
    }

    .search-panel__input {
        width: 300px;
        padding: 10px 20px 15px;
        border: 1px solid #ccc;
        border-radius: 3px;
        background: #fff;
        font-size: 16.799px;
    }

    .search-panel__sort {
        display: inline-block;
        width: 51%;
        margin-right: -4px;
        text-align: right;
        vertical-align: middle;
    }

    nav li, nav ul {
        margin: 0;
        padding: 0;
        list-style: none;
    }

    .panel-options {
        display: inline-block;
        margin-right: -4px;
        margin-left: 28px;
        font-size: 14.399px;
        list-style-type: none;
        vertical-align: middle;
    }

    .panel-options__item:first-child {
        margin-left: 0;
    }

    .panel-options__link {
        display: block;
        position: relative;
        padding-bottom: 5px;
        color: #3a3f6d;
        transition: color .3s ease;
    }

        .panel-options__link--active, .panel-options__link:hover {
            color: #282828;
            text-decoration: none;
        }

            .panel-options__link--active:after, .panel-options__link:hover:after {
                opacity: 1!important;
            }

        .panel-options__link:after {
            content: '';
            display: block;
            position: absolute;
            right: 0;
            bottom: 0;
            left: 0;
            height: 3px;
            background: #3a3f6d;
            opacity: 0;
            transition: opacity .3s ease;
        }

    .panel-options__item {
        display: inline-block;
        margin-right: -4px;
        margin-left: 22px;
        vertical-align: middle;
    }

    .search-panel__bottom {
        padding: 13px 0;
        background: #f7f7f7;
    }

    .search-panel__label--wide {
        width: 12%;
    }

    .search-panel__label {
        display: inline-block;
        margin: 0 -4px 0 0;
        font-size: 14.399px;
        font-weight: 700;
        text-transform: uppercase;
        vertical-align: middle;
    }

    .search-panel__description {
        padding: 12px 0;
        border-top: 1px solid #e2e2e1;
        background: #f7f7f7;
        font-size: 14.399px;
        line-height: 1.3;
        text-align: center;
    }

    .painter-short {
        padding: 18px 0;
        border-top: 1px solid #e2e2e1;
        overflow: hidden;
    }

    .painter-short a {
        color: #ab8b3f;
        text-decoration: none;
        background-color: transparent;
    }

    .painter-short a:hover {
        text-decoration: underline;
    }

    .painter-short__info {
        position: relative;
        width: 540px;
    }

        .painter-short__info:nth-child(n) {
            float: left;
            margin-right: 20px;
            clear: none;
        }

        .painter-short__info:before {
            content: '';
            position: absolute;
            top: 0;
            right: -80px;
            bottom: 0;
            border-left: 1px dotted #ccc;
        }

    .painter-short__name {
        margin-top: 0;
        margin-bottom: 5px;
        font-size: 21.6px;
        font-weight: 600;
        letter-spacing: 0.24px;
    }

    .painter-short__location {
        margin: 2px 0;
        font-size: 14.399px;
    }

    .painter-short__info-icon {
        display: inline-block;
        max-width: 18px;
        max-height: 16px;
        margin-right: 3px;
        vertical-align: bottom;
    }

    .painter-short__mail, .painter-short__phone {
        display: inline-block;
        margin: 0 29px 2px 0;
        font-size: 13.2px;
        letter-spacing: 0.24px;
        vertical-align: middle;
    }

    .painter-short__description {
        margin-top: 5px;
        font-size: 15.6px;
        font-weight: 200;
        line-height: 1.5;
    }

    .painter-short__logo {
        width: 139px;
    }

        .painter-short__logo:nth-child(n) {
            float: left;
            margin-right: 20px;
            clear: none;
        }

        .painter-short__logo:nth-child(12n+1) {
            clear: left;
        }

    .painter-short__logo-img {
        height: 25px;
        width: 25px;
        max-width: 100%;
    }

    img {
        max-width: 100%;
        height: auto;
        border: 0;
        vertical-align: middle;
    }

    .painter-short__actions {
        position: relative;
        padding: 5px 0 20px;
        margin-left: 79px !important;
        width: 139px;
    }

        .painter-short__actions:nth-child(n) {
            float: left;
            margin-right: 20px;
            clear: none;
        }

        .painter-short__actions:last-child, .painter-short__actions:nth-child(12n) {
            margin-right: 0;
        }

    .painter-short {
        padding: 18px 0;
        border-top: 1px solid #e2e2e1;
        overflow: hidden;
    }

    .painter-icons__item {
        display: inline-block;
        width: 33.3%;
        margin-right: -4px;
        text-align: center;
        vertical-align: middle;
    }

    .painter-icons__icon {
        display: block;
        max-width: 18px;
        max-height: 16px;
        margin: 0 auto 8px;
    }

    .panel-tags {
        margin-left: -9px;
        list-style-type: none;
    }

    .panel-tags__item {
        margin-top: 5px;
        margin-left: 9px;
    }

    .panel-tags, .panel-tags__item {
        display: inline-block;
        margin-right: -4px;
        vertical-align: middle;
    }

    .tag-item {
        display: inline;
        position: relative;
        vertical-align: middle;
        margin-left: 7px;
    }

    .tag-item:first-child {
        margin-left: 0;
    }

    .tag-item:first-child:before {
        content: "";
    }

    .tag-item:before {
        content: "\00b7";
        padding-right: 4px;
        vertical-align: baseline;
        font-weight: bolder;
    }

    .certificate {
        font-family: myriad-pro, "Myriad Pro", "Helvetica Neue", Helvetica, sans-serif;
        font-weight: 700;
        color: rgb(79,79,79);
    }

    ul {
        margin: 0;
        padding: 0;
        list-style-position: inside;
    }


    .button {
      display: inline-flex;
      height: 25px;
      min-width: 100px;
      border: 2px solid #BFC0C0;
      padding: 5px;
      color: #BFC0C0;
      text-decoration: none;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }

    a {
      color: rgb(40,40,40);
      text-decoration: none;
    }

    a:link {
        -webkit-tap-highlight-color: rgba(0,0,0,.1);
    }

    #button-anim {
      position: relative;
      overflow: hidden;
      cursor: pointer;
    }

    #button-anim a {
      position: relative;
      transition: all .45s ease-Out;
    }

    #translate {
      transform: rotate(50deg);
      width: 100%;
      height: 250%;
      left: -200px;
      top: -30px;
      background-color: #F4F200;
      background-image: -moz-linear-gradient(top, #fff 0%, #F4F200 100%); 
      background-image: -webkit-linear-gradient(top, #fff 0%,#F4F200 100%); 
      background-image: linear-gradient(to bottom, #fff 0%,#F4F200 100%);
      background-repeat: no-repeat;
      background-position: 0%;
      position: absolute;
      transition: all .3s ease-Out;
    }

    .button-anim-active #translate {
      left: 0;
    }

    .button-anim-active a {
      color: #2D3142;
    }

    .button-neon {
      background-color: white;
      border: 2px solid #a5a7a7;
      border-radius: 5px;
      -webkit-transition: all .15s ease-in-out;
      transition: all .15s ease-in-out;
      color: #00d7c3;
    }
    .button-neon:hover {
      box-shadow: 0 0 10px 0 #F4F200 inset, 0 0 20px 2px #F4F200;
      border: 2px solid #F4F200;
    }

    .search-panel__search {
        position: absolute;
        top: 15px;
        right: 15px;
        border: 0;
    }
</style>

