<template>
  <div class="slide">
   <img :src="slide.src" />
  </div>
</template>

<script>
export default {
  name: 'ImageView',
  props: {
    slide: Object
  }
}
</script>

<style scoped>
  img {
    width: 100%;
  }
  .slide {
    display: inline-block;
  }
</style>
