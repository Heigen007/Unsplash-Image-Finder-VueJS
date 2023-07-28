<template>
    <div class="py-5">
        <div class="container">
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-3 g-3">
                <div v-for="(photo, index) in formattedPhotoArray" :key="index" class="col">
                    <RouterLink :to="{ name: 'PhotoView', params: { id: photo.id } }">
                        <PhotoComponent :url="photo.url" />
                    </RouterLink>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>

import PhotoComponent from "./PhotoComponent";
import axios from 'axios';

export default {
    inject: ['unsplashURL', 'unsplashAccessKey'],
    props: ['currentSearch', 'isFavouritesPhotoes'],
    data: function(){
        return {
            photoArray: []
        }
    },
    components: {
        PhotoComponent
    },
    watch: {
        currentSearch: {
            handler: function (newSearch) {
                if(newSearch)
                    this.searchPhotosByString(newSearch);
            },
            immediate: true
        }
    },
    mounted() {
        if (this.isFavouritesPhotoes) {
            this.getFavouritePhotoes()
        } else {
            this.getRandomPhotoes()
        }
    },
    methods: {
        getFavouritePhotoes(){
            this.photoArray = JSON.parse(localStorage.getItem('favouritePhotoes')) || [];
        },
        async getRandomPhotoes(){
            const response = await axios.get(this.unsplashURL + '/photos/random?count=2', {
                headers: {
                    Authorization: unsplashAccessKey,
                },
            });
            this.photoArray = response.data;
        },
        async searchPhotosByString(newSearch){
            const response = await axios.get(this.unsplashURL + `/search/photos?query=${newSearch}&per_page=2`, {
                headers: {
                    Authorization: unsplashAccessKey,
                },
            });
            this.photoArray = response.data.results;
        }
    },
    computed: {
        formattedPhotoArray(){
            if(this.photoArray.length > 0){
                return this.photoArray.map((photo) => {
                    return {
                        id: photo.id,
                        url: photo.url || photo.urls?.full
                    }
                })
            }
            return [];
        }
    }
};
</script>