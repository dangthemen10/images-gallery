<template>
  <div class="container px-8 mb-4">
    <h2 class="text-2lx font-semibold text-center mt-4">
      Upload an image to Cloudinary
    </h2>
    <div class="grid md:grid-cols-6 grid-cols-1">
      <div class="md:col-start-2 md:col-span-4 shadow-lg">
        <div class="grid md:grid-cols-2 grid-cols-1">
          <div class="blog p-6 rounded-lg bg-white max-w-sm mt-4">
            <form>
              <div class="form-group mb-6">
                <div>
                  <label
                    class="form-label inline-block mb-2 text-gray-700"
                    for="file-input"
                    >Upload Image</label
                  >
                  <input
                    id="file-input"
                    type="file"
                    class="
                      form-control
                      block
                      w-full
                      px-3
                      py-1.5
                      text-base
                      font-normal
                      text-gray-700
                      border border-solid border-gray-300
                      rounded
                    "
                    accept="image/png, image/jpg, image/jpeg"
                    @change="handleFileChange($event)"
                  />
                </div>
              </div>
              <button
                class="
                  w-full
                  px-6
                  py-2.5
                  bg-green-600
                  text-white
                  font-medium
                  text-xs
                  rounded
                  shadow-md
                "
                :class="uploadStatus && 'bg-gray-600'"
                :disabled="uploadStatus"
                type="submit"
                @click.prevent="uploadImage"
              >
                {{ uploadStatus ? 'Uploading' : 'Upload' }}
              </button>
            </form>
          </div>
          <!--Image Preview-->
          <div class="p-4">
            <h3 class="text-semibold text-center mb-2">Image Preview</h3>
            <AlertMessage v-if="error" :message="error" />
            <img
              v-if="filePreview"
              class="bg-white p-1 border rounded"
              :src="filePreview"
            />
            <p v-if="fileSize">{{ fileSize }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import AlertMessage from '@/components/AlertMessage.vue'
export default {
  components: {
    AlertMessage
  },

  data() {
    return {
      file: null,
      filePreview: null,
      fileSize: 0.0,
      uploadStatus: false,
      error: null,
      router: this.$route
    }
  },
  methods: {
    handleFileChange(e) {
      this.error = null
      this.file = e.target.files[0]
      this.getImagePreviews(this.file)
    },
    formatBytes(size, decimals = 2) {
      if (size === 0) return '0 bytes'
      const k = 1024
      const dm = decimals < 0 ? 0 : decimals
      const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB']
      const i = Math.floor(Math.log(size) / Math.log(k))
      return parseFloat((size / Math.pow(k, i)).toFixed(dm)) + ' ' + sizes[i]
    },
    getImagePreviews(image) {
      if (/\.(jpe?g|png)$/i.test(image.name) && image.size < 1000000) {
        const reader = new FileReader()
        reader.onloadend = (e) => {
          this.filePreview = e.target.result
          this.fileSize = this.formatBytes(image.size)
        }
        reader.readAsDataURL(image)
      } else {
        this.error = 'FIle is not supported for size bigger than 1MB'
        this.filePreview = null
        this.fileSize = null
      }
    },
    submitUpload(f) {
      if (!this.file) return
      const reader = new FileReader()
      reader.readAsDataURL(this.file)
      reader.onloadend = () => {
        this.uploadImage(reader.result)
      }
    },
    uploadImage() {
      // eslint-disable-next-line no-console
      console.log('formData', this.file)
      this.uploadStatus = true
      const formData = new FormData()
      if (this.file) formData.append('file', this.file)
      this.$axios
        .post('/api/v1/image', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        .then((response) => {
          // eslint-disable-next-line no-console
          console.log('response', response)
          if (response.data.data.url) {
            this.error = null
            this.uploadStatus = false
            this.$router.push('/')
          }
        })
        .catch((error) => {
          this.error = error.message
          this.uploadStatus = false
        })
    }
  }
}
</script>
