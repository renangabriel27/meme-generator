<template>
  <div class="container p-3">
    <h1 class="text-center">{{ title }}</h1>

    <div class="d-block m-auto text-center position-relative main-content">
      <div class="my-3">
        <p class="fixed-top text">
          {{ text.top }}
        </p>

        <img :src="image.path"
             :alt="image.title"
             :title="image.title"
             class="main-content">

        <p class="fixed-bottom text">
          {{ text.bottom }}
        </p>
      </div>

      <meme-gallery :gallery="gallery" />

      <div>
        <div class="form-group">
          <label>Top text</label>
          <input type="text" class="form-control" v-model="text.top">
        </div>

        <div class="form-group">
          <label>Bottom text</label>
          <input type="text" class="form-control" v-model="text.bottom">
        </div>

        <button class="btn btn-default" @click="resetInputs()">
          Reset
        </button>
      </div>
    </div>
  </div>
</template>

<script>

import MemeGallery from './MemeGallery';

export default {
  name: 'MemeGenerator',

  components: { MemeGallery },

  data() {
    return {
      title: 'Meme Generator',
      image: {
        path: '',
        title: '',
      },
      text: {
        top: '',
        bottom: '',
      },
      gallery: [
        { title: 'Nerd', path: 'images/nerd.png' },
        { title: 'Chapolin', path: 'images/chapolin.png' },
      ],
    };
  },

  mounted() {
    this.setImage();
    this.onUpdateImage();
  },

  methods: {
    setImage() {
      this.updateImage(this.gallery[0]);
    },

    onUpdateImage() {
      this.$root.$on('update-image', (image) => {
        this.updateImage(image);
      });
    },

    resetInputs() {
      this.text.top = '';
      this.text.bottom = '';
    },

    updateImage(image) {
      this.image.path = image.path;
      this.image.title = image.title;
    },
  },
}
</script>

<style scoped>

.main-content {
  width: 600px;
  height: 500px;
  word-wrap: break-word;
}

.text {
  color: #fff;
  font-family: 'Passion One';
  font-size: 40px;
  line-height: 40px;
  padding: 5px;
  position: absolute;
  text-shadow: 3px 3px 3px #000;
}

</style>
