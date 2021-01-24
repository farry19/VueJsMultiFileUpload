<template>
  <div
    class="App min-w-screen min-h-screen flex flex-col bg-grey-lightest font-sans"
  >
    <uploader />
  </div>
</template>

<script>
import Uploader from "../components/Uploader.vue";

export default {
  components: {
    Uploader,
  },
};
</script>
