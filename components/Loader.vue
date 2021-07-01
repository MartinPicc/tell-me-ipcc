<template>
    <div id="loader">
        <Lottie
            :width="300"
            :height="200"
            :options="lottieOptions"
            @animCreated="handleAnimation"
        />
    </div>
</template>

<script>
import Lottie from "vue-lottie/src/lottie.vue";
import * as animationData from "~/assets/lego_loader.json";

export default {
  components: {
    Lottie,
  },
  data() {
    return {
      anim: null, // for saving the reference to the animation
      lottieOptions: { animationData: animationData.default },
    };
  },
  methods: {
    handleAnimation: function(anim) {
      this.anim = anim;
      this.anim.setSpeed(3.4);
    }
  }
};
</script>
