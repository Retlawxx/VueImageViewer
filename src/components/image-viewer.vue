<template>
  <div class="image-viewer">
    <div>
      <button @click="goToPrev">Previous</button>
      <button @click="goToNext">Next</button>
    </div>
    <div class="slides-inner">
      <transition-group name="imageView" tag="ImageView">
        <image-view class="imageView"
                      v-for="(slide, index) in slides"
                      v-if="isImageViewVisible(index)"
                      :key="slide.src"
                      :slide="slide" />
      </transition-group>
    </div>
  </div>
</template>
<script>
import ImageView from '../components/image-view'
export default {
  name: 'ImageViewer',
  data () {
    return {
      slides: [
        {
          src: 'static/img/Agropoli.jpg'
        },
        {
          src: 'static/img/Cicerale.jpg'
        },
        {
          src: 'static/img/Cicerale2.jpg'
        }
      ],
      currentIndex: 0
    }
  },
  components: {
    ImageView
  },
  methods: {
    isImageViewVisible: function (index) {
      return Math.abs(this.currentIndex) % this.slides.length === index
    },
    goToPrev () {
      this.currentIndex--
    },
    goToNext () {
      this.currentIndex++
    }
  }
}
</script>

<style scoped>
  button {
    padding: 0.5em;
  }
  .image-viewer {
    max-width: 30em;
    width: 100%;
  }
  .slides-inner {
    margin-top: 1em;
    position: relative;
  }
  .imageView {
    transition: all 0.5s;
    position: absolute;
  }
  .imageView-enter, .imageView-leave-to {
    opacity: 0;
  }
  .imageView-enter-to {
    opacity: 1;
  }
  .imageView-move {
    opacity: 1;
    transition: all 0.5s;
  }
</style>
