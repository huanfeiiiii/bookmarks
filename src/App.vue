<script setup>
import { ref, watchEffect } from "vue";

const items = ref(null);

watchEffect(async () => {
    const url = "src/assets/links.json";
    items.value = await (await fetch(url)).json();
});

function getScrollTop() {
    var scrollPos;
    if (window.pageYOffset) {
        scrollPos = window.pageYOffset;
    } else if (document.compatMode && document.compatMode != "BackCompat") {
        scrollPos = document.documentElement.scrollTop;
    } else if (document.body) {
        scrollPos = document.body.scrollTop;
    }
    return scrollPos;
}

document.addEventListener("scroll", function () {
    const goTop = document.querySelector(".go-top")
    if (getScrollTop() >= 300) {
        goTop.style.display = "block"
    } else {
        goTop.style.display = "none"
    }
})

</script>

<template>
    <div class="link">
        <div class="link_nav">
            <a class="link_author" href="/">
                <img src="https://cravatar.cn/avatar/5ada90a1b56e60cc13ef5aa0581044c2" alt="">
                <p>导航</p>
            </a>
            <div class="nav_items">
                <a :href="'#'+item.class_name" class="nav_item" v-for="item in items">{{item.class_name}}</a>
            </div>
        </div>
        <div class="link_items">
            <div class="link_item" :id="item.class_name" v-for="item in items">
                <div class="item_title">
                    {{ item.class_name }}
                    <a :href="'#'+item.class_name" :title="item.class_name" class="hash-link">#</a>
                </div>
                <div class="links">
                    <a :href="link.link" class="link" target="_blank" :title="link.desc"
                        v-for="link in item.class_links">
                        <div class="title">
                            <img :src="link.logo" alt="" v-if="link.logo != ''">
                            <img src="https://favicon.rss.ink/v1/aHR0cHM6Ly90b29saWdodC5jbg==" alt="" v-else>
                            <div class="name">{{ link.name }}</div>
                        </div>
                        <div class="desc" v-if="link.desc">{{ link.desc }}</div>
                    </a>
                </div>
            </div>
        </div>
    </div>

    <a class="go-top fa-solid fa-arrow-up fa-fw show" href="#app"></a>
</template>

<style lang="stylus" scoped>
@import '../src/assets/link_items.styl'
@import '../src/assets/utils.styl'
</style>
