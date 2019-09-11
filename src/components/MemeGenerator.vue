<template>
  <div class="row m-0 p-2">
    <div class="col-md-6 text-center mb-4">
      <div id="main-content" class="d-block mx-auto position-relative">
        <p class="fixed-top text" :style="textColor">
          {{ text.top }}
        </p>

        <img :src="image.src"
             :alt="image.title"
             :title="image.title"
             id="main-image">

        <p class="fixed-bottom text" :style="textColor">
          {{ text.bottom }}
        </p>
      </div>
    </div>

    <div class="col-md-6">
      <h1 v-if="showTitle" class="text-center">
        <small>{{ title }}</small>
      </h1>

      <div class="form-group">
        <input
          type="text"
          placeholder="Top text"
          class="form-control"
          v-model="text.top"
        >
      </div>

      <div class="form-group">
        <input
          type="text"
          placeholder="Bottom text"
          class="form-control"
          v-model="text.bottom"
        >
      </div>

      <div class="form-group">
        <div class="custom-control custom-checkbox" @click="toggleTextUppercase()">
          <input
            type="checkbox"
            class="custom-control-input"
            :checked="text.uppercase"
          >
          <label class="custom-control-label">UPPERCASE</label>
        </div>
      </div>

      <div class="form-group">
        <label>TEXT COLOR</label>
        <span v-for="(color, index) in colors" :key="index">
          <span class="ml-2 border border-primary"
                :style="colorStyle(color)"
                @click="updateTextColor(color)"></span>
        </span>
      </div>

      <div class="form-group">
        <input
          class="form-control-file"
          accept="image/*"
          type="file"
          @change="previewImage($event)"
        >
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

import html2canvas from 'html2canvas';
import FileSaver from 'file-saver';

export default {
  name: 'MemeGenerator',

  data() {
    return {
      title: 'Meme Generator',
      image: {
        src: 'images/i-have-no-idea.png',
        title: 'Dog',
      },
      text: {
        top: '',
        bottom: '',
        color: 'white',
        uppercase: true,
      },
      colors: ['red', 'black', 'white'],
      showTitle: false,
    };
  },

  computed: {
    textColor() {
      const uppercase = this.text.uppercase ? 'uppercase' : 'lowercase';
      return { color: this.text.color, textTransform: uppercase };
    },
  },

  methods: {
    colorStyle(color) {
      return {
        width: '16px',
        height: '16px',
        borderRadius: '50%',
        display: 'inline-block',
        background: color,
        cursor: 'pointer'
      };
    },

    resetInputs() {
      this.text.top = '';
      this.text.bottom = '';
      this.text.color = 'white';
    },

    updateImage(image) {
      this.image.src = image.src;
      this.image.title = image.title;
    },

    updateTextColor(color) {
      this.text.color = color;
    },

    previewImage(event) {
      let input = event.target;

      if (input.files && input.files[0]) {
        let render = new FileReader();

        render.onload = (e) => {
          this.image.src = e.target.result;
        };

        render.readAsDataURL(input.files[0]);
      }
    },

    printImageToCanvas() {
      return html2canvas(document.querySelector('#main-content'));
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

#main-content {
  word-wrap: break-word;
}

#main-image {
  width: 100%;
  height: auto;
  max-height: 800px;
}

.text {
  color: #fff;
  font-family: 'Passion One';
  font-size: 4.5vw;
  line-height: 3.8vw;
  padding: 5px;
  position: absolute;
  text-transform: uppercase;
  text-shadow: 3px 3px 3px #000;
}

</style>
