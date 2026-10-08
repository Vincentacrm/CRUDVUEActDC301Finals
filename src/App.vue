<script setup>
import { ref, onMounted } from 'vue'
import BoardingHouseItem from './components/BoardingHouseItem.vue'

const boardingHouses = ref([])

const newBoardingHouse = ref({
  name: '',
  location: '',
  price: '',
  roomType: '',
  availability: 'Available'
})

function addBoardingHouse() {
  if (
    newBoardingHouse.value.name.trim() === '' ||
    newBoardingHouse.value.location.trim() === '' ||
    newBoardingHouse.value.price === '' ||
    newBoardingHouse.value.roomType.trim() === ''
  ) {
    return
  }

  boardingHouses.value.push({
    id: Date.now(),
    name: newBoardingHouse.value.name,
    location: newBoardingHouse.value.location,
    price: newBoardingHouse.value.price,
    roomType: newBoardingHouse.value.roomType,
    availability: newBoardingHouse.value.availability
  })

  newBoardingHouse.value = {
    name: '',
    location: '',
    price: '',
    roomType: '',
    availability: 'Available'
  }

  saveBoardingHouses()
}

function deleteBoardingHouse(id) {
  boardingHouses.value = boardingHouses.value.filter(
    boardingHouse => boardingHouse.id !== id
  )

  saveBoardingHouses()
}

function updateBoardingHouse(updatedBoardingHouse) {
  const index = boardingHouses.value.findIndex(
    boardingHouse => boardingHouse.id === updatedBoardingHouse.id
  )

  if (index !== -1) {
    boardingHouses.value[index] = updatedBoardingHouse
  }

  saveBoardingHouses()
}

function saveBoardingHouses() {
  localStorage.setItem(
    'boardingHouses',
    JSON.stringify(boardingHouses.value)
  )
}

onMounted(() => {
  const savedBoardingHouses = localStorage.getItem('boardingHouses')

  if (savedBoardingHouses) {
    boardingHouses.value = JSON.parse(savedBoardingHouses)
  }
})
</script>

<template>
  <div>

    <h1>Boarding House Listing</h1>

    <h2>Add Boarding House</h2>

    <input
      v-model="newBoardingHouse.name"
      type="text"
      placeholder="Boarding house name"
    />

    <input
      v-model="newBoardingHouse.location"
      type="text"
      placeholder="Location"
    />

    <input
      v-model="newBoardingHouse.price"
      type="number"
      placeholder="Price"
    />

    <input
      v-model="newBoardingHouse.roomType"
      type="text"
      placeholder="Room type"
    />

    <select v-model="newBoardingHouse.availability">
      <option value="Available">Available</option>
      <option value="Full">Full</option>
    </select>

    <button @click="addBoardingHouse">
      Add
    </button>

    <h2>Boarding Houses</h2>

    <div>

      <BoardingHouseItem
        v-for="boardingHouse in boardingHouses"
        :key="boardingHouse.id"
        :boardingHouse="boardingHouse"
        @delete="deleteBoardingHouse"
        @update="updateBoardingHouse"
      />

    </div>

  </div>
</template>