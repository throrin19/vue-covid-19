<template>
    <v-app>
        <drawer />
        <v-app-bar
            app
            clipped-left>
            <v-app-bar-nav-icon />
            <v-toolbar-title>Vue COVID 19</v-toolbar-title>
        </v-app-bar>
        <v-content>
            <router-view />
        </v-content>
    </v-app>
</template>

<script>
import Drawer from './components/Drawer.vue';

export default {
    name : 'App',
    components : {
        Drawer,
    },
};
</script>
