<template>
    <HeaderComponent />
    <router-view />
    <ScrollToTopButton />
</template>

<script>
import HeaderComponent from "./components/HeaderComponent";
import ScrollToTopButton from './components/ScrollToTopButton.vue';

export default {
    name: "App",
    components: {
        HeaderComponent,
        ScrollToTopButton
    },
    provide() {
        // глобальные конфигурационные переменные
        return {
            unsplashURL: 'https://api.unsplash.com',
            unsplashAccessKey: process.env.unsplashAccessKey
        }
    }
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}

.btn-warning{
    background-color: #fff200;
}

.custom-mt-80 {
    margin-top: 80px;
}
</style>
