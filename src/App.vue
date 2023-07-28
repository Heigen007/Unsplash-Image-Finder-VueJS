<template>
    <HeaderComponent />
    <router-view />
</template>

<script>
import HeaderComponent from "./components/HeaderComponent";

export default {
    name: "App",
    components: {
        HeaderComponent
    },
    provide() {
        // глобальные конфигурационные переменные
        return {
            unsplashURL: 'https://api.unsplash.com',
            unsplashAccessKey: 'Client-ID YOUR_KEY'
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

.mt-80 {
    margin-top: 80px;
}
</style>
