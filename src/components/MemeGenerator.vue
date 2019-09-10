<template>
  <div class="row m-2">
    <div class="col-md-6 text-center mb-4">
      <div id="capture" class="d-block mx-auto position-relative main-content">
        <p class="fixed-top text" :style="textColor">
          {{ text.top }}
        </p>

        <img :src="image.path"
             :alt="image.title"
             :title="image.title"
             id="main-image"
             class="img-fluid">

        <p class="fixed-bottom text" :style="textColor">
          {{ text.bottom }}
        </p>
      </div>

      <div v-if="showGallery">
        <meme-gallery :gallery="gallery" />

          <button class="btn btn-outline-danger mt-2"
                  @click="updateShowGallery(false)">
            Close Meme Templates
          </button>
      </div>
      <div v-else>
        <button class="btn btn-outline-primary mt-2"
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

      <div class="form-group">
        <label>Color text</label>
        <input type="text"
               :style="inputColorStyle"
               class="form-control"
               v-model="text.color">
      </div>

      <div class="form-group">
        <label>Text shadow</label>
        <input type="text"
               :style="inputShadowStyle"
               class="form-control"
               v-model="text.shadow">
      </div>

      <div class="form-group">
        <div class="custom-control custom-checkbox" @click="toggleTextUppercase()">
          <input type="checkbox" class="custom-control-input" :checked="text.uppercase">
          <label class="custom-control-label">UPPERCASE</label>
        </div>
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
        color: '#fff',
        shadow: '#000',
        uppercase: true,
      },
      gallery: [
        { title: 'Dog', path: 'images/i-have-no-idea.png' },
        { title: 'Skeleton', path: 'images/skeleton.png' },
      ],
      showGallery: true,
    };
  },

  computed: {
    textColor() {
      return {
        color: this.text.color,
        textTransform: this.text.uppercase ? 'uppercase' : 'lowercase',
        textShadow: `3px 3px 3px ${this.text.shadow}`
      };
    },

    inputColorStyle() {
      return { border: `1px solid ${this.text.color}` };
    },

    inputShadowStyle() {
      return { border: `1px solid ${this.text.shadow}` };
    },
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
      this.text.color = '#fff';
      this.text.shadow = '#000';
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

    toggleTextUppercase() {
      this.text.uppercase = !this.text.uppercase;
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
  text-transform: uppercase;
}

</style>
