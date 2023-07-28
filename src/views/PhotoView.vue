<template>
    <div id="background-picture"></div>
    <div class="container py-5 mt-80">
        <div class="row">
            <div class="col-md-6 col-6">
                <!-- Information about the owner of the photo -->
                <div class="d-flex owner-info">
                    <!-- Owner's photo -->
                    <img :src="photo.user?.profile_image?.medium" alt="Owner's Photo" class="rounded border border-white border-1 mr-3" style="width: 40px; height: 40px;">
                    <!-- Owner's name and username in a column -->
                    <div class="ms-2 d-flex flex-column align-items-start">
                        <h6 class="mb-0 text-white text-start">{{ photo.user?.name }}</h6>
                        <h6 class="mb-0 text-white custom-font-size-sm text-start">{{ photo.user?.username }}</h6>
                    </div>
                </div>
            </div>
            <div class="col-md-6 col-6">
                <div class="d-flex justify-content-end">
                    <!-- Heart button -->
                    <button class="btn btn-light btn-md mr-3 border border-1 border-gray" @click="savePhotoToFavourites">
                        <i class="bi bi-heart"></i>
                    </button>
                    <!-- Download button -->
                    <button class="btn btn-warning btn-md ms-2" @click="downloadPhoto">
                        <i class="bi bi-download"></i>
                        <label class="ms-1 custom-download-label">Download</label>
                    </button>
                </div>
            </div>
        </div>

        <!-- Main photo -->
        <div class="mt-4">
            <img :src="photo.urls?.full" alt="Photo" class="img-fluid rounded">
        </div>

        <SuccesfullAlert v-if="showAlert" @close="showAlert = false">
            Photo added to favorites successfully!
        </SuccesfullAlert>
    </div>
</template>

<script>
import axios from 'axios';
import SuccesfullAlert from '../components/SuccesfullAlert.vue';

export default {
    name: "PhotoView",
    inject: ['unsplashAccessKey'],
    data: function(){
        return {
            photo: {},
            showAlert: false,
        }
    },
    created() {
        this.getPhotoById(this.$route.params.id);
    },
    components: {
        SuccesfullAlert
    },
    methods: {
        async getPhotoById(photoId){
            const response = await axios.get('https://api.unsplash.com/photos/' + photoId, {
                headers: {
                    Authorization: this.unsplashAccessKey, // 'Client-ID
                },
            });
            this.photo = response.data;
        },
        savePhotoToFavourites(){
            var favouritePhotoes = JSON.parse(localStorage.getItem('favouritePhotoes')) || [];

            if(favouritePhotoes.some(photo => photo.id === this.photo.id)){
                favouritePhotoes = favouritePhotoes.filter(photo => photo.id !== this.photo.id);
            }
            
            favouritePhotoes.unshift({
                id: this.photo.id,
                url: this.photo.links.download
            });
            localStorage.setItem('favouritePhotoes', JSON.stringify(favouritePhotoes));
            this.showAlert = true;
        },
        downloadPhoto() {
            window.open(this.photo.links.download, '_blank');
        },
    }
};
</script>

<style scoped>
#background-picture{
  position: fixed;
  top: -25vh;
  left: 0;
  width: 100%;
  height: 100%;
  background: url('../assets/photoPageBackground.png') no-repeat center center fixed; 
  background-size: cover;
  -webkit-filter: blur(2px) brightness(0.7);
  filter: blur(2px) brightness(0.7);
  z-index: -1;
}

.owner-info p {
    margin-top: 0;
}

.custom-font-size-sm {
    font-size: 0.75rem;
}

@media (max-width: 768px) {
    #background-picture, .custom-download-label {
        display: none;
    }
    .owner-info h6 {
        color: black !important;
    }
}
</style>