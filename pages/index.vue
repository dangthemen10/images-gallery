<template>
  <div
    class="
      container
      px-5
      py-2
      mx-auto
      lg:pt-12 lg:px-32
      bg-white
      dark:bg-slate-900
    "
  >
    <div class="flex flex-wrap -m-1 md:-m-2">
      <div
        v-for="image in images"
        :key="image.public_id"
        class="flex flex-wrap md:w-1/4 w-1/2"
      >
        <div class="w-full p-1 md:p-2">
          <img
            alt="gallery"
            class="block object-cover object-center w-full h-full rounded-lg"
            :src="image.secure_url"
          />
        </div>
      </div>
    </div>
    <div v-if="next" class="text-center my-4">
      <button
        class="
          bg-green-700
          px-4
          py-2
          text-center text-white
          border
          rounded
          shadow-sm
        "
        :disabled="disabled"
        @click="loadMore"
      >
        Load more
      </button>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      const { data: response } = await $axios.get('/api/v1/images')
      const images = response.data.images
      let next
      if (response.data.next_cursor) {
        next = response.data.next_cursor
      }
      return {
        images,
        next
      }
    } catch (error) {
      // eslint-disable-next-line no-console
      console.log(error)
    }
  },
  data() {
    return {
      images: [],
      next: null
    }
  },
  mounted() {
    this.fetchImages()
  },
  methods: {
    async fetchImages() {
      try {
        const response = await this.$axios.get('/api/v1/images')
        this.images = response.data.images
        if (response.data.next_cursor) {
          this.next = response.data.next_cursor
        }
      } catch (error) {
        // eslint-disable-next-line no-console
        console.log(error)
      }
    },
    async loadMore() {
      const params = new URLSearchParams()
      if (this.next) {
        params.append('next_cursor', this.next)
        const response = await this.$axios.get(`/api/v1/images?${params}`)
        if (response.data) {
          for (let i = 0; i < response.data.images.length; i++) {
            this.images.push(response.data.images[i])
          }
          if (response.data.next_cursor) {
            this.next = response.data.next_cursor
            params.delete('next_cursor')
          } else {
            params.delete('next_cursor')
            this.next = null
          }
        }
      }
    }
  }
}
</script>
