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
          class="md:flex mx-6 h-full md:mx-auto max-w-lg md:max-w-2xl h-64 my-4"
        >
          <div class="relative md:w-1/4 bg-white rounded-l-lg">
            <input
              type="file"
              @change="uploadFile($event, index)"
              class="absolute h-full w-full opacity-0"
            />
            <div
              class="h-full w-full object-cover rounded-l-lg"
              :style="{
                'background-image': `url(${
                  file.preview ? file.preview : 'https://placehold.it/150x115'
                })`,
              }"
              alt="bag"
            ></div>
          </div>
          <div
            class="w-full h-full md:w-3/4 px-4 py-4 bg-white rounded-r-lg flex"
          >
            <div class="w-full h-full flex flex-col">
              <a
                @click="removeFile(index)"
                class="mb-3 cursor-pointer mb-4 block self-end text-white"
              >
                <svg
                  class="w-4 h-4"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                >
                  <path
                    fillRule="evenodd"
                    d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                    clipRule="evenodd"
                  />
                </svg>
              </a>
              <textarea
                v-model="file.description"
                class="border w-full h-full rounded p-2 focus:outline-none"
              ></textarea>
            </div>
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
    removeFile(index) {
      this.files.splice(index, 1);
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
