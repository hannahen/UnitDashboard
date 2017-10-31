<template>
    <div>
        <div v-for="(menu, index) in menuLinks">
            <slide-menu
                    :class="menu.customClass"
                    :title="menu.name"
                    :active="index === activeIndex"
                    :menu="menu"
                    :collapsed="collapsed"
                    :open="openIndex === index"
                    :mobile="mobile"
                    @open="doOpen(index)"
                    @action="doAction(menu)"
                    :static="menu.static"
            ></slide-menu>
        </div>
    </div>
</template>

<script>
    export default {
        props:    ['menuItems', 'collapsed', 'mobile'],
        data() {
            let activeUrl = window.location.pathname.replace(/\?.*/, '');

            let activeIndex = _.findIndex(this.menuItems, {url: activeUrl});

            if (activeIndex < 0) {
                for (let [index, item] of this.menuItems.entries()) {
                    if (item.links.length) {
                        if (_.findIndex(item.links, {url: activeUrl}) >= 0){
                            activeIndex = index;
                            break;
                        }
                    }
                }
            }
            return {
                menuLinks: this.menuItems,
                activeIndex: activeIndex,
                openIndex: activeIndex
            };
        },
        created() {

        },
        mounted() {
        },

        watch:    {
        },
        methods: {
            doAction(menu) {
                if (this.collapsed) {
                    this.$emit('collapse');
                } else if(menu.url){
                    document.location = menu.url;
                }
            },
            doOpen(index){
                if(this.openIndex === index){
                    this.openIndex = -1;
                } else {
                    this.openIndex = index;
                }
            }
        }
    }
</script>
