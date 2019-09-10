<template>
  <div>
    <h1>{{ title }}</h1>

    <img :src="image.path"
         :alt="image.title"
         :title="image.title">
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
