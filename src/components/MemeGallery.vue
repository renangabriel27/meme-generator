<template>
  <div id="meme-gallery" class="border">
    <span v-for="(image, index) in gallery" :key="index">
      <img :src="image.path"
           :title="image.title"
           class="image-gallery"
           @click="emitUpdateImage(image)">
    </span>
  </div>
</template>

<script>

export default {
  name: 'MemeGallery',

  props: {
    gallery: {
      type: Array,
      required: true
    },
  },

  methods: {
    emitUpdateImage(image) {
      this.$root.$emit('update-image', image);
    },
  },
};

</script>

<style scoped>

#meme-gallery {
  height: 90px;
  overflow: scroll;
  overflow-y: hidden;
  white-space: nowrap;
}

.image-gallery {
  width: 65px;
  height: 100%;
  cursor: pointer;
}

</style>
