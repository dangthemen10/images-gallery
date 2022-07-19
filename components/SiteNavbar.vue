<template>
  <nav
    class="
      flex
      items-center
      justify-between
      flex-wrap
      bg-emerald-400
      py-4
      lg:px-12
      shadow
      border-solid border-t-2 border-emerald-700
    "
  >
    <div
      class="
        flex
        justify-between
        lg:w-auto
        w-full
        lg:border-b-0
        pl-6
        pr-2
        border-solid border-b-2 border-gray-300
        pb-5
        lg:pb-0
      "
    >
      <div class="flex items-center flex-shrink-0 text-white mr-20">
        <span class="font-semibold text-2xl tracking-tight"
          >Picture Gallery</span
        >
      </div>
      <div class="block lg:hidden">
        <button
          id="nav"
          class="
            flex
            items-center
            px-3
            py-2
            border-2
            rounded
            text-blue-700
            border-blue-700
            hover:text-blue-700 hover:border-blue-700
          "
        >
          <svg
            class="fill-current h-3 w-3"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <title>Menu</title>
            <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z" />
          </svg>
        </button>
      </div>
    </div>

    <div
      class="
        menu
        w-full
        lg:block
        flex-grow
        lg:flex lg:items-center lg:w-auto lg:px-3
        px-8
      "
    >
      <div class="text-lg font-bold text-white lg:flex-grow">
        <nuxt-link
          to="/"
          class="
            block
            bg-emerald-400
            mt-4
            lg:inline-block lg:mt-0
            hover:text-white
            px-4
            py-2
            font-bold
            rounded
            hover:bg-emerald-500
            mr-2
          "
        >
          Gallery
        </nuxt-link>
      </div>
      <div class="flex">
        <nuxt-link
          to="/upload"
          class="
            block
            bg-emerald-400
            text-lg
            px-4
            ml-2
            py-2
            rounded
            text-white
            font-bold
            hover:text-white
            mt-4
            hover:bg-emerald-500
            lg:mt-0
          "
        >
          Upload
        </nuxt-link>
      </div>
      <div class="flex">
        <DarkModeToggle />
      </div>
    </div>
  </nav>
</template>
<script>
import DarkModeToggle from '@/components/DarkModeToggle.vue'
export default {
  components: {
    DarkModeToggle
  }
}
</script>
