<template>
  <div class="container p-3">
    <h1 class="text-center">{{ title }}</h1>

    <div class="d-block m-auto text-center position-relative main-content">
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
        path: 'images/nerd.png',
        title: 'Nerd'
      },
      text: {
        top: '',
        bottom: '',
      },
    };
  },

  methods: {
    resetInputs() {
      this.text.top = '';
      this.text.bottom = '';
    }
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
