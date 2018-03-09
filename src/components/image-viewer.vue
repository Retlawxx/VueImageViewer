<template>
  <div class="image-viewer">
    <div id="classificationResults">
      Hotdogs: <br /> <img class="classificationResultImage"
                    :src="classification.src" 
                    v-if="classification.hotdog == true"
                    v-for="classification in classifications" 
                    :key="classification.index" />
      <br />
      Not Hotdogs: <br /> <img class="classificationResultImage"
                    :src="classification.src" 
                    v-if="classification.hotdog == false"
                    v-for="classification in classifications" 
                    :key="classification.index" />
    </div>
    <div id="errorScore">
      Dein Fehler-Score: {{userErrorScore}}
    </div>
    <div>
      <button @click="goToPrev">Previous</button>
      <button @click="goToNext">Next</button>
      <button @click="classifyImage(true)">This is a Hotdog!</button>
      <button @click="classifyImage(false)">This is NOT a Hotdog!</button>
    </div>
    <div class="slides-inner">
     <p>current image index: {{currentIndex}} </p>
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
          src: 'static/img/random/Agropoli.jpg'
        },
        {
          src: 'static/img/random/Cicerale.jpg'
        },
        {
          src: 'static/img/random/Cicerale2.jpg'
        },
        {
          src: 'static/img/random/nothotdog1.jpg'
        },
        {
          src: 'static/img/random/nothotdog2.jpg'
        },
        {
          src: 'static/img/hotdogs/hotdog.jpg',
          hotdog: true
        },
        {
          src: 'static/img/hotdogs/hotdog1.jpg',
          hotdog: true
        },
        {
          src: 'static/img/hotdogs/hotdog2.jpg',
          hotdog: true
        }
      ],
      classifications: [],
      currentIndex: 0,
      userErrorScore: 0
    }
  },
  components: {
    ImageView
  },
  methods: {
    isImageViewVisible: function (index) {
      // return Math.abs(this.currentIndex) % this.slides.length === index
      return this.currentIndex === index;
    },

    goToPrev () {
      this.currentIndex--
      if(this.currentIndex < 0 ) {
        this.currentIndex = 0;
      }
    },
    goToNext () {
      this.currentIndex++
      if(this.currentIndex >= this.slides.length) {
          this.currentIndex = this.slides.length - 1;
      }
    },
    classifyImage(hotdog = false) {
      // deep copy to prevent 
      const currentSlideSelected = JSON.parse(JSON.stringify(this.slides[this.currentIndex]));

      if(hotdog && !currentSlideSelected.hotdog
        || !hotdog && currentSlideSelected.hotdog) {
        this.userErrorScore++;
      }

      // check if hotdog already inside 
      let existingClassification = null;
      this.classifications.forEach(classification => {
          if(classification.src == currentSlideSelected.src) {
              existingClassification = classification
          }
      });

      if(!existingClassification) {
        currentSlideSelected.hotdog = hotdog;
        this.classifications.push(currentSlideSelected);

      } else {
        existingClassification.hotdog = hotdog;
      }

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
  #classificationResults {
    margin-top: 1em;
    margin-bottom: 1em;
  }
  .classificationResultImage {
    width: 5em;
    padding: 1em;
  }
</style>
