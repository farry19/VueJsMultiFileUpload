<template>
  <div class="content-wrapper pt-1 flex-1 bg-grey-light">
    <div class="content py-4 container max-w-md mx-auto">
      <div class="flex justify-end">
        <button
          @click="addFiles"
          class="cursor-pointer px-2 py-1 text-xs border border-black rounded"
        >
          +
        </button>
      </div>
      <template v-for="(file, index) in files">
        <div
          :key="`file-${index}`"
          class="md:flex mx-6 md:mx-auto max-w-lg md:max-w-2xl h-64 my-4"
        >
          <div class="relative md:w-1/3 bg-white rounded-l-lg">
            <input
              type="file"
              @change="uploadFile($event, index)"
              class="absolute h-full w-full opacity-0"
            />
            <div
              class="h-full w-full object-cover rounded-l-lg"
              :style="{
                'background-image': `url(${
                  file.preview ? file.preview : 'https://placehold.it/200x250'
                })`,
              }"
              alt="bag"
            ></div>
          </div>
          <div class="w-full md:w-2/3 px-4 py-4 bg-white rounded-r-lg flex">
            <textarea
              class="border w-full rounded px-4 py-10 focus:outline-none"
            ></textarea>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>
<script>
import axios from "axios";

const tmpFile = {
  data: null,
  preview: null,
};
export default {
  name: "Upload",
  data() {
    return {
      previewImage: null,
      files: [],
    };
  },
  mounted() {
    // :style="files[index].preview ? 'background-image': `url(${files[index].preview})` : `url('https://ik.imagekit.io/q5edmtudmz/FB_IMG_15658659197157667_wOd8n5yFyXI.jpg')`"
    this.files.push({ ...tmpFile });
  },
  methods: {
    addFiles() {
      this.files.push({ ...tmpFile });
    },
    loadPreview(file, index) {
      let reader = new FileReader();
      reader.onload = (e) => {
        this.files[index].preview = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    uploadFile(event, index) {
      this.files[index].data = event.target.files[0];
      this.loadPreview(event.target.files[0], index);
      // this.files[index].data = event.target.files[0];
    },
    handleSubmit() {
      const formData = new FormData();
      for (const i of Object.keys(this.files)) {
        formData.append("files", this.files[i]);
      }
      axios
        .post("http://localhost:4000/api/file-upload", formData, {})
        .then((res) => {
          console.log(res);
        });
    },
  },
};
</script>
