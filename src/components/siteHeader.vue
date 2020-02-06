<template>
    <header role="banner" class="siteHeader">
        <nav class="header__top">
            <div class="container">
                <div class="header__find-painter">Tekijäpankki</div>
                <a href="/" title="Home" class="header-logo">
                    <img title="Painters Bank" class="header-logo__icon" src="../content/logo.jpg" />
                </a>
            </div>
        </nav>
        <nav class="header__bottom">
            <div class="container">
                <ul class="header-menu">
                    <li class="header-menu__item">
                        <a href="/" class="header-menu__link">
                            <img src="https://img.icons8.com/flat_round/64/000000/home--v1.png" class="header-menu__home">
                        </a>
                    </li>
                    <li class="header-menu__item"><a href="/Home" class="header-menu__link">Tekijät</a></li>
                </ul>
            </div>
        </nav>
    </header>
</template>

<script>
    export default {
        name: 'SiteHeader'
    }
</script>

<style scoped>
    .siteHeader {
        position: relative;
        color: #fff;
        transition: -webkit-filter .4s;
        transition: filter .4s;
        transition: filter .4s,-webkit-filter .4s;
    }

    .header__top {
        background: #fff;
        background: linear-gradient(180deg,#fff 0,#fff 29%,#fdfdfd 37%,#e7e6e6 86%,#e2e2e2);
    }

    .header__find-painter {
        float: left;
        padding: 10px 0;
        color: #867d67;
        font-size: 20.4px;
        font-weight: 700;
        letter-spacing: 0.12px;
        text-transform: uppercase;
        margin-left: 150px;
    }

    a {
        color: #ab8b3f;
        text-decoration: none;
        background-color: transparent;
    }

    a:link {
        -webkit-tap-highlight-color: rgba(0,0,0,.1);
    }

    a:hover {
        background-color: #b1a487;
    }

    svg:not(:root) {
        overflow: hidden;
    }

    .header-logo {
        position: absolute;
        top: 0;
        left: 0;
    }

    .header-logo__icon {
        width: 100px;
        height: 100px;
    }

    nav li, nav ul {
        margin: 0;
        padding: 0;
        list-style: none;
    }

    .header__bottom {
        padding: 17px 0;
        background: #867d67;
    }

    .header-menu {
        float: left;
        font-size: 15.6px;
        font-size: 1.3rem;
        letter-spacing: 0.48px;
        letter-spacing: .04rem;
        text-transform: uppercase;
        margin-left: calc(99.99% * 0.166666666666667 - 16.66666666666666px + 20px) !important;
    }

    .header-menu__item {
        display: inline-block;
        margin-right: -4px;
        vertical-align: middle;
    }

    .header-menu__link {
        display: block;
        padding: 13px 19px;
        color: inherit;
        transition: background .3s ease;
    }

    .header-menu__home {
        width: 50px;
        height: 50px;
    }

    .header-menu__link {
        display: block;
        padding: 13px 19px;
        color: inherit;
        transition: background .3s ease;
    }
</style>