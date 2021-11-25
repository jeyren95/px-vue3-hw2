<template>
  <div class="relative flex flex-col">
    <div
    class="
      group
      block
      w-full
      rounded-lg
      bg-gray-100
      focus-within:ring-2
      focus-within:ring-offset-2
      focus-within:ring-offset-gray-100
      focus-within:ring-pink-500
      overflow-hidden
    "
    >
      <img :src="imageUrl" class="object-cover pointers-event-none group-hover:opacity-75 h-48 w-full" alt />
    </div>

    <div class="flex-1 flex md:flex-col justify-between items-start md:items-stretch gap-3 px-2">
      <div class="mt-1 flex-1">
        <div class="flex justify-between items-center gap-3">
          <div>
            RM 
            <span class="text-2xl font-bold">{{ price }}</span>
          </div>
          <div v-if="availability !== 'single-item'" class="text-sm text-gray-500">
            {{ numOfStock }} piece available
          </div>
          <span v-else class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-red-100 text-red-800">
            Only One
          </span>
        </div>
      </div>

      <p class="block text-sm font-medium text-gray-900 truncate pointer-events-none">
        {{ title }}
      </p>
      
      <p class="block text-sm font-medium text-gray-500 pointer-events-none">
        {{ description }}
      </p>


      <div class="flex flex-col md:flex-row gap-3 py-3">
        <Button 
        type="button" 
        class="border-transparent  text-white bg-pink-600 hover:bg-pink-700"
        @click="handleFetchItem"
        >
          <PencilIcon class="h-5 w-5 mr-2" />  
          Edit
        </Button>
        <Button 
        type="button" 
        class="border-pink-500 text-pink-500 bg-white hover:text-pink-700"
        @click="handleDeleteItem"
        >
          <TrashIcon class="h-5 w-5 mr-2" />
          Delete
        </Button>
      </div>
    </div>

  </div>
</template>


<script>
import Button from "./Button.vue"
import { TrashIcon, PencilIcon } from "@heroicons/vue/solid"


export default {
  components: {
    Button,
    TrashIcon,
    PencilIcon
  },
  props: ["id", "title", "imageUrl", "description", "price", "availability", "numOfStock", "condition"],
  methods: {
    handleDeleteItem() {
      this.$emit("delete-item", this.id)
    },
    handleFetchItem() {
      this.$emit("fetch-item", this.id)
    }
  }
}

</script>