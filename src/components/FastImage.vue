<template lang="html">
  <div class="bg_image_wrap">
    <div 
      :style="{backgroundImage: this.relatedImageUrl, backgroundPosition: this.bgPosition}" 
    ></div>
  </div>
</template>

<script>
import closestNumber from "closest-number";
export default {
  name: "fast-image",
  props: {
    imageHash: {
      required: true
    },
    bgPosition: {
      type: String,
      required: false,
      default: "top"
    }
  },
  data() {
    return {
      width: 0
    };
  },
  computed: {
    avaliableScreenSizes() {
      return Object.keys(this.imageHash);
    },
    closestParentWindowWidth() {
      if (this.avaliableScreenSizes.length == 0 || this.parentWidth == null)
        return 0;
      else return closestNumber(this.avaliableScreenSizes, this.parentWidth);
    },
    relatedImageUrl() {
      var related_img = this.imageHash[this.closestParentWindowWidth][
        this.orient
      ];
      if (
        this.imageHash == undefined ||
        related_img == undefined ||
        this.switchDelay > 0
      )
        return "url('')";
      else return 'url("' + related_img + '")';
    },
    orient() {
      let ratio = window.innerWidth / window.innerHeight;
      if (ratio >= 2.3) return "wide";
      if (ratio >= 1.7) return "landscape";
      else return "portrait";
    },
    parentWidth: {
      get() {
        return this.width;
      },
      set(newWidth) {
        this.width = newWidth;
      }
    }
  },
  mounted() {
    this.$nextTick(function() {
      this.parentWidth = this.$el.clientWidth;
    });
  }
};
</script>

<style>
.bg_image_wrap {
  position: relative;
  width: 100%;
  height: 100%;
}
.bg_image_wrap .bg_image {
  width: 100%;
  height: 100%;
  position: absolute;
  background-size: cover;
  background-repeat: no-repeat;
  opacity: 1;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s;
}

.fade-enter,
.fade-leave-to {
  opacity: 1;
}
</style>