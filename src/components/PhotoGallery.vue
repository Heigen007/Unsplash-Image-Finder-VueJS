<template>
    <div class="py-5">
        <div class="container">
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-3 g-3">
                <div v-for="(photo, index) in photoArray" :key="index" class="col">
                    <RouterLink :to="{ name: 'PhotoView', params: { id: photo.id } }">
                        <PhotoComponent :url="photo.links.download" />
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
    props: ['currentSearch'],
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
                this.searchPhotosByString(newSearch);
            },
            immediate: true
        }
    },
    async created() {
        const response = await axios.get('https://api.unsplash.com/photos/random?count=2', {
            headers: {
                Authorization: 'YOUR_ID',
            },
        });
        this.photoArray = response.data;
    },
    methods: {
        async searchPhotosByString(newSearch){
            const response = await axios.get(`https://api.unsplash.com/search/photos?query=${newSearch}&per_page=2`, {
                headers: {
                    Authorization: 'YOUR_ID',
                },
            });
            this.photoArray = response.data.results;
        }
    }
};
</script>