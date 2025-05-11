<template>
  <div id="gallery" class="photo-wall">
    <a
      v-for="(image, key) in data.images"
      :key="key"
      :href="image.largeURL"
      :data-pswp-width="image.width"
      :data-pswp-height="image.height"
      target="_blank"
      rel="noreferrer"
    >
      <img :src="image.thumbnailURL" :alt="'Image ' + (key + 1)" />
    </a>
  </div>
</template>

<script setup>
import PhotoSwipeLightbox from "photoswipe/lightbox";
import { ref } from "vue";
import { onSlideEnter, onSlideLeave, useIsSlideActive } from "@slidev/client";
import "photoswipe/style.css";
let lightbox = ref(null);
const data = ref({
  images: [
    {
      largeURL:
        "https://cdn.photoswipe.com/photoswipe-demo-images/photos/1/img-2500.jpg",
      thumbnailURL:
        "https://cdn.photoswipe.com/photoswipe-demo-images/photos/1/img-200.jpg",
      width: 1875,
      height: 2500,
    },
    {
      largeURL:
        "https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-2500.jpg",
      thumbnailURL:
        "https://cdn.photoswipe.com/photoswipe-demo-images/photos/2/img-200.jpg",
      width: 1669,
      height: 2500,
    },
    {
      largeURL:
        "https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-2500.jpg",
      thumbnailURL:
        "https://cdn.photoswipe.com/photoswipe-demo-images/photos/3/img-200.jpg",
      width: 2500,
      height: 1666,
    },
  ],
});

onSlideEnter(() => {
  lightbox.value = new PhotoSwipeLightbox({
    gallery: "#gallery",
    children: "a",
    pswpModule: () => import("photoswipe"),
  });
  lightbox.value?.init();
});
onSlideLeave(() => {
  lightbox.value?.destroy();
  lightbox.value = null;
});
</script>

<style>
.photo-wall {
  width: 50vw;
  height: 50vh;
}
</style>
