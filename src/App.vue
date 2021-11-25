<template>
  <main class="bg-gray-50 lg:flex">
    <div class="flex-1">
      <div class="max-w-7xl mx-auto pt-16 pb-24 px-4 sm:px-6 lg:px-8">
        <div class="sm:flex sm:flex-col sm:align-center mb-12">
          <h1 class="text-5xl font-extrabold text-gray-900 sm:text-center">Marketplace</h1>
        </div>
        <div class="flex justify-center items-center mb-5">
          <Button 
          type="button"
          class="border-transparent  text-white bg-pink-600 hover:bg-pink-700 mr-5"
          @click="() => page > 1 && page--"
          >
            <ArrowCircleLeftIcon class="h-5 w-5 mr-2" />
            Previous
          </Button>
          <span class="text-gray-900 sm:text-center">Page {{ page }}</span>
          <Button 
          type="button"
          class="border-transparent  text-white bg-pink-600 hover:bg-pink-700 ml-5"
          @click="() => page++"
          >
            Next
            <ArrowCircleRightIcon class="h-5 w-5 ml-2" />
          </Button>
        </div>
        <div v-if="items.length > 0" class="grid md:grid-cols-2 gap-x-4 gap-y-8 xl:grid-cols-3 xl:gap-x-6">
          <Item 
          v-for="(item, index) in items"
          :id="item._id"
          :key="index"
          :title="item.title"
          :imageUrl="item.imageUrl"
          :price="item.price"
          :description="item.description"
          :availability="item.availability"
          :numOfStock="item.availability !== 'single-item' && item.numOfStock"
          :condition="item.condition"
          @delete-item="deleteItem"
          @fetch-item="fetchItem"
          />
        </div>
        <h1 v-else>Loading items...</h1>
      </div>
    </div>

    <div class="flex-initial bg-white w-full lg:max-w-md border-b border-gray-100">
      <form class="flex flex-col h-full" @submit.prevent="handleSubmit">
        <div class="py-6 px-4 bg-pink-700 sm:px-6">
          <div class="flex items-center justify-between">
            <h2 v-if="formState === 'add'" class="text-lg font-medium text-white">New Listing</h2>
            <h2 v-else class="text-lg font-medium text-white">Edit Listing</h2>
          </div>
          <div class="mt-1">
            <p v-if="formState === 'add'" class="text-sm text-pink-300">Get started by filling in the information below to create your new listing.</p>
            <p v-else class="text-sm text-pink-300">Get started by filling in the information below to create your new listing.</p>
          </div>
        </div>

        <div class="px-4 sm:px-6 pb-12">
          <div class="space-y-6 pt-6 pb-5">
            <!-- form inputs -->
            <!-- in vue3, v-model translates into :value="title" @update:model-value="setTitle(title)" -->
            <InputField 
            label="Title"
            id="title"
            type="text"
            name="title"
            v-model="title"
            required="true"
            />
            <InputField 
            label="Price"
            id="price"
            type="text"
            name="price"
            v-model.number="price"
            required="true"
            />
            <TextAreaField 
            label="Description"
            id="description"
            name="description"
            v-model="description"
            required="true"
            />
            <SelectField 
            label="Condition"
            id="condition"
            name="condition"
            :options="conditionOptions"
            v-model="condition"
            required="true"
            />
            <SelectField 
            label="Availability"
            id="availability"
            name="availablility"
            :options="availabilityOptions"
            v-model="availability"
            required="true"
            />        
            <InputField 
            v-if="availability === 'in-stock'"
            label="Number of Available Stock"
            type="text"
            id="numOfStock"
            name="numOfStock"
            v-model.number="numOfStock"
            required="true"
            />    
          </div>
        </div>

        <div v-if="formState === 'add'" class="flex-shrink-0 px-4 py-4 flex justify-end border-t border-gray-200">
          <Button 
          type="submit"
          class="ml-4 border-transparent text-white bg-pink-600 hover:bg-pink-700"
          >
            ADD
          </Button>
        </div>  

        <div v-else class="flex-shrink-0 px-4 py-4 flex justify-end border-t border-gray-200">
          <Button 
          type="submit"
          class="mr-4 border-transparent text-white bg-pink-600 hover:bg-pink-700"
          >
            EDIT
          </Button>
          <Button 
          type="button"
          class="border-pink-500 text-pink-500 bg-white hover:text-pink-700"
          @click="() => {
            formState = 'add'
            resetFormInputs()  
          }"
          >
            CANCEL
          </Button>
        </div>         
      </form>
    </div>


  </main>
</template>

<script>
import Item from "./components/Item.vue"
import Button from "./components/Button.vue"
import InputField from "./components/InputField.vue"
import TextAreaField from "./components/TextAreaField.vue"
import SelectField from "./components/SelectField.vue"
import { ArrowCircleLeftIcon, ArrowCircleRightIcon } from "@heroicons/vue/solid"

export default {
  name: 'App',
  data() {
    return {
      items: [],
      page: 1, 
      formState: "add",
      controller: undefined,
      title: "",
      itemId: "",
      price: "",
      imageUrl: "https://images.unsplash.com/photo-1594995846645-d58328c3ffa4?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=543&h=384&q=8",
      description: "",
      condition: "new",
      availability: "in-stock",
      numOfStock: "",
      conditionOptions: [
        { display: "New", value: "new" }, 
        { display: "Used (like new)", value: "used_like-new" },
        { display: "Used (good)", value: "used_good" },
        { display: "Used (fair)", value: "used_fair" }
      ],
      availabilityOptions: [
        { display: "In Stock", value: "in-stock" },
        { display: "Single Item", value: "single-item" }
      ]
    }
  },
  components: {
    Item,
    Button,
    InputField,
    TextAreaField,
    SelectField,
    ArrowCircleLeftIcon,
    ArrowCircleRightIcon
  },
  methods: {
    resetFormInputs() {
      this.title = ""
      this.price = ""
      this.itemId = ""
      this.description = ""
      this.condition = "new"
      this.imageUrl = "https://images.unsplash.com/photo-1594995846645-d58328c3ffa4?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=543&h=384&q=8"
      this.availability = "in-stock"
      this.numOfStock = ""
    },
    fetchItems() {
      this.controller = new AbortController()
      const response = fetch(`https://ecomm-service.herokuapp.com/marketplace?limit=6&page=${this.page}`, { signal: this.controller.signal })
      .then((res) => res.json())

      return response
    },
    fetchItem(id) {
      this.formState = "edit"

      fetch(`https://ecomm-service.herokuapp.com/marketplace/${id}`)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title
        this.price = data.price
        this.description = data.description
        this.condition = data.condition
        this.imageUrl = data.imageUrl
        this.availability = data.availability 
        this.numOfStock = data.numOfStock 
        this.itemId = data._id
      })
      .catch((err) => console.log(err))
    },
    editItem() {
      fetch(`https://ecomm-service.herokuapp.com/marketplace/${this.itemId}`, {
        method: "PATCH",
        headers: {
          accept: "application/json",
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          title: this.title,
          condition: this.condition,
          description: this.description,
          availability: this.availability,
          imageUrl: this.imageUrl,
          numOfStock: this.numOfStock, 
          price: this.price,
          category: "string"
        })
      })
      .then(this.fetchItems)
      .then((data) => {
        this.items = data
        this.formState = "add"
      })
      .catch((err) => console.log(err))
    },
    deleteItem(id) {
      fetch(`https://ecomm-service.herokuapp.com/marketplace/${id}`, {
        method: "DELETE",
        headers: {
          accept: "application/json"
        }
      })
      .then(this.fetchItems)
      .then((data) => this.items = data)
      .catch((err) => console.log(err))
    },
    addItem() {
      fetch(`https://ecomm-service.herokuapp.com/marketplace`, {
        method: "POST",
        headers: {
          accept: "application/json",
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          title: this.title,
          price: this.price,
          description: this.description,
          condition: this.condition,
          availability: this.availability,
          numOfStock: this.availability === 'single-item' ? 1 : this.numOfStock,
          imageUrl: this.imageUrl     
        })
      })
      .then(this.fetchItems)
      .then((data) => this.items = data)
      .catch((err) => console.log(err))
    },
    handleSubmit() {
      if (this.formState === "add") {
        this.addItem()
      } else {
        this.editItem()
      }

      this.formState = "add"
      this.resetFormInputs()
    }
  },
  mounted() {
    this.fetchItems()
    .then((data) => {
      console.log(data)
      this.items = data
    })
    .catch((err) => console.log(err))
  },
  watch: {
    page() {
      this.fetchItems()
      .then((data) => this.items = data)
      .catch((err) => console.log(err))
    }
  }
}
</script>

