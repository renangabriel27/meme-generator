<template>
  <div class="container p-3">
    <h1 class="text-center">{{ title }}</h1>

    <div id="main-div" class="d-block m-auto text-center">
      <img :src="image.path"
           :alt="image.title"
           :title="image.title">
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
    };
  },
}
</script>

<style scoped>

#main-div {
  width: 600px;
  height: 500px;
}

</style>
