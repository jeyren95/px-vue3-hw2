<template>
  <main class="bg-gray-50 lg:flex">
    <div class="flex-1">
      <div class="max-w-7xl mx-auto pt-16 pb-24 px-4 sm:px-6 lg:px-8">
        <div class="sm:flex sm:flex-col sm:align-center mb-12">
          <h1 class="text-5xl font-extrabold text-gray-900 sm:text-center">Marketplace</h1>
        </div>
        <div>
          <Button 
          type="button"
          class="border-transparent  text-white bg-pink-600 hover:bg-pink-700"
          >
            <ArrowCircleLeftIcon class="h-5 w-5 inline-block" />
            Previous
          </Button>
          <span class="text-gray-900 sm:text-center">Page {{ page }}</span>
          <Button 
          type="button"
          class="border-transparent  text-white bg-pink-600 hover:bg-pink-700"
          >
            <ArrowCircleRightIcon class="h-5 w-5 inline-block" />
            Next
          </Button>
        </div>
        <div v-if="items.length > 0" class="grid md:grid-cols-2 gap-x-4 gap-y-8 xl:grid-cols-3 xl:gap-x-6">
          <Item 
          v-for="(item, index) in items"
          :key="index"
          :title="item.title"
          :imageUrl="item.imageUrl"
          :price="item.price"
          :description="item.description"
          :availability="item.availability"
          :numOfStock="item.availability !== 'single-item' && item.numOfStock"
          :condition="item.condition"
          />
        </div>
        <h1 v-else>Loading items...</h1>
      </div>
    </div>

    <div class="flex-initial bg-white w-full lg:max-w-md border-b border-gray-100">

    </div>


  </main>
</template>

<script>
import Item from "./components/Item.vue"
import { ArrowCircleLeftIcon, ArrowCircleRightIcon } from "@heroicons/vue/solid"

export default {
  name: 'App',
  data() {
    return {
      items: [],
      page: 1
    }
  },
  components: {
    Item,
    ArrowCircleLeftIcon,
    ArrowCircleRightIcon
  },
  methods: {
    fetchItems(page, signal) {
      const response = fetch(`https://ecomm-service.herokuapp.com/marketplace?limit=6&page=${page}`, {signal})
      .then((res) => res.json())

      return response
    }
  },
  mounted() {
    const controller = new AbortController()

    this.fetchItems(this.page, controller.signal)
    .then((data) => this.items = data)
    .catch((err) => console.log(err))

    return () => controller.abort()
  },
  updated() {
    const controller = new AbortController()

    this.fetchItems(this.page, controller.signal)
    .then((data) => this.items = data)
    .catch((err) => console.log(err))

    return () => controller.abort()
  }
}
</script>

