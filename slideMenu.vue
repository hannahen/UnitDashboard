<template>
    <div class="section">
        <div :class="{active: active || open, noSubNav: !hasLinks}" class="primary ripple" @click="toggleOpen">
            <a @click="menuAction"><i class="menu-icon" :class="menu.icon"></i></a>
            <a @click="menuAction" v-if="!collapsed">{{ title }}
                <template v-if="hasLinks && !mobile">
                    <i class="fa menu-toggle" :class="{'fa-angle-up': !open, 'fa-angle-down': open}"></i>
                </template>
            </a>
        </div>
            <ul :class="{'open-menu': open && !collapsed}">
                <li v-for="link in menu.links" :class="{active: link.url === activeUrl}">
                    <a :href="link.url" :target="link.target">{{ link.title }}</a>
                </li>
            </ul>
    </div>
</template>

<script>
    export default {
        props:    ['menu', 'title', 'collapsed', 'open', 'active', 'mobile', 'static'],
        data() {
            return {
                activeUrl: window.location.pathname.replace(/\?.*/, '')
            };
        },
        created() {
        },
        mounted() {
        },
        computed: {
            icon: function() {
                return this.menu.icon;
            },
            url: function() {
                return this.menu.url;
            },
            hasLinks: function(){
                return Object.keys(this.menu.links).length > 0;
            },
        },
        watch:    {
        },
        methods: {
            toggleOpen: function (){
                if(!this.static){
                    this.$emit('open');
                } else {
                    this.$emit('action');
                }
            },
            menuAction: function (){
                this.$emit('action');
            }
        }
    }
</script>
