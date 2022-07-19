<template>
  <div class="flex flex-col h-screen">
    <SiteNavbar />
    <main class="flex-grow">
      <div
        id="app"
        class="bg-auto bg-white dark:bg-black"
        :class="dark ? 'dark' : 'light'"
      >
        <Nuxt />
      </div>
    </main>
    <SiteFooter />
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import SiteNavbar from '@/components/SiteNavbar.vue'
import SiteFooter from '@/components/SiteFooter.vue'
export default {
  components: {
    SiteNavbar,
    SiteFooter
  },
  computed: {
    ...mapGetters(['dark'])
  }
}
</script>
