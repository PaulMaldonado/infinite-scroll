<template>
  <div>
      <div class="container mt-4">
          <div class="row">
              <h2 class="text-center mt-4 display-4 text-dark fw-bold">Scroll infinito</h2>
              <div class="col-md-4 col-sm-12 col-lg-4 col-xl-4 col-xxl-4 mt-3" v-for="photo in photos" :key="photo.id">
                <div class="card shadow-lg">
                    <img :src="photo.thumbnailUrl" alt="" class="card-img-top img-fluid">

                    <div class="card-body">
                        <h5 class="card-title">
                            {{ photo.title }}
                        </h5>
                    </div>
                </div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Photos',

    data() {
        return {
            photos: [],
        }
    },

    created() {
        this.showPhotos()
    },

    mounted() {
        this.scroll()
    },

    methods: {
        showPhotos() {
            setTimeout(() => {
                axios.get('https://jsonplaceholder.typicode.com/photos?_start=0&_limit=30')
                    .then(response => {
                        console.log(response)

                        this.photos = response.data
                    })
                    .catch(error => {
                        console.error(error)
                    })
            }, 2000)
        },

        scroll() {
            window.onscroll = () => {
                let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;

                if(bottomOfWindow) {
                    setTimeout(() => {
                        axios.get('https://jsonplaceholder.typicode.com/photos')
                            .then(response => {
                                console.log(response)

                                this.photos = response.data
                            })
                    }, 2000)
                }
            }
        }
    }
}
</script>

<style scoped>

</style>