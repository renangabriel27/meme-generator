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

      <div id="meme-gallery" class="border">
        <span v-for="(image, index) in gallery" :key="index">
          <img :src="image.path"
               :title="image.title"
               class="image-gallery"
               @click="updateImage(image)">
        </span>
      </div>

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
export default {
  name: 'MemeGenerator',

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
  },

  methods: {
    setImage() {
      this.updateImage(this.gallery[0]);
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

#meme-gallery {
  height: 90px;
  overflow: scroll;
  overflow-y: hidden;
  white-space: nowrap;
}

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

.image-gallery {
  width: 65px;
  height: 100%;
  cursor: pointer;
}

</style>
