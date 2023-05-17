<template>
  <div>
    <input v-model="query" type="text" placeholder="搜索图片">
    <button @click="searchImages">搜索</button>
    <div>
      <div v-for="image in images" :key="image.id">
        <img :src="'http://farm' + image.farm + '.static.flickr.com/' + image.server + '/' + image.id + '_' + image.secret + '.jpg'" alt="">
      </div>
    </div>
    <button v-if="page < totalPages" @click="getMoreImages">获取更多图像</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      query: '',
      images: [],
      page: 1,
      perPage: 20,
      totalPages: null,
    };
  },
  methods: {
    searchImages() {
      axios
        .get(
          `https://api.flickr.com/services/rest/?method=flickr.photos.search&api_key=41090645badd835c00d51eb32c0630ec&format=json&nojsoncallback=1&safe_search=1&text=${this.query}&page=${this.page}&per_page=${this.perPage}`
        )
        .then(response => {
          this.images = response.data.photos.photo;
          this.totalPages = response.data.photos.pages;
        })
        .catch(error => {
          console.error(error);
        });
    },
    getMoreImages() {
      this.page++;
      axios
        .get(
          `https://api.flickr.com/services/rest/?method=flickr.photos.search&api_key=41090645badd835c00d51eb32c0630ec&format=json&nojsoncallback=1&safe_search=1&text=${this.query}&page=${this.page}&per_page=${this.perPage}`
        )
        .then(response => {
          this.images = this.images.concat(response.data.photos.photo);
        })
        .catch(error => {
          console.error(error);
        });
    },
  },
};
</script>
