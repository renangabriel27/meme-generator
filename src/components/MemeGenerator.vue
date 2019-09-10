<template>
  <div class="row m-2">
    <div class="col-md-6 text-center mb-4">
      <div id="capture" class="d-block mx-auto position-relative main-content">
        <p class="fixed-top text">
          {{ text.top }}
        </p>

        <img :src="image.path"
             :alt="image.title"
             :title="image.title"
             id="main-image"
             class="img-fluid">

        <p class="fixed-bottom text">
          {{ text.bottom }}
        </p>
      </div>

      <div v-if="showGallery">
        <meme-gallery :gallery="gallery" />

          <button class="btn btn-outline-danger"
                  @click="updateShowGallery(false)">
            Close Meme Templates
          </button>
      </div>
      <div v-else>
        <button class="btn btn-outline-primary"
                @click="updateShowGallery(true)">
          View Meme Templates
        </button>
      </div>
    </div>

    <div class="col-md-6">
      <h1 class="text-center">{{ title }}</h1>

      <div class="form-group">
        <label>Top text</label>
        <input type="text" class="form-control" v-model="text.top">
      </div>

      <div class="form-group">
        <label>Bottom text</label>
        <input type="text" class="form-control" v-model="text.bottom">
      </div>

      <button class="btn btn-outline-primary" @click="resetInputs()">
        Reset
      </button>

      <button class="btn btn-outline-danger ml-2" @click="download()">
        Download
      </button>
    </div>
  </div>
</template>

<script>

import MemeGallery from './MemeGallery';
import html2canvas from 'html2canvas';
import FileSaver from 'file-saver';

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
        { title: 'Dog', path: 'images/i-have-no-idea.png' },
        { title: 'Skeleton', path: 'images/skeleton.png' },
      ],
      showGallery: true,
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

    updateShowGallery(value) {
      this.showGallery = value;
    },

    printImageToCanvas() {
      return html2canvas(document.querySelector('#capture'));
    },

    async download() {
      const canvas = await this.printImageToCanvas();
      window.open(canvas.toBlob(function(blob) {
        FileSaver.saveAs(blob, 'meme.png')
      }), '_self');
    },
  },
}
</script>

<style scoped>

.main-content {
  width: 500px;
  height: 500px;
  word-wrap: break-word;
}

#main-image {
  width: 100%;
  height: 100%;
}

.text {
  color: #fff;
  font-family: 'Passion One';
  font-size: 40px;
  line-height: 40px;
  padding: 5px;
  position: absolute;
  text-shadow: 3px 3px 3px #000;
  text-transform: uppercase;
}

</style>
