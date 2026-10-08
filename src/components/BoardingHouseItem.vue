<script setup>
import { ref } from 'vue'

const props = defineProps({
  boardingHouse: Object
})

const emit = defineEmits([
  'delete',
  'update'
])

const isEditing = ref(false)

const editedBoardingHouse = ref({
  name: props.boardingHouse.name,
  location: props.boardingHouse.location,
  price: props.boardingHouse.price,
  roomType: props.boardingHouse.roomType,
  availability: props.boardingHouse.availability
})

function updateBoardingHouse() {
  if (
    editedBoardingHouse.value.name.trim() === '' ||
    editedBoardingHouse.value.location.trim() === '' ||
    editedBoardingHouse.value.price === '' ||
    editedBoardingHouse.value.roomType.trim() === ''
  ) {
    return
  }

  emit('update', {
    ...props.boardingHouse,
    ...editedBoardingHouse.value
  })

  isEditing.value = false
}

function deleteBoardingHouse() {
  emit('delete', props.boardingHouse.id)
}

function startEditing() {
  editedBoardingHouse.value = {
    name: props.boardingHouse.name,
    location: props.boardingHouse.location,
    price: props.boardingHouse.price,
    roomType: props.boardingHouse.roomType,
    availability: props.boardingHouse.availability
  }

  isEditing.value = true
}
</script>

<template>

  <div>

    <div v-if="!isEditing">

      <p>
        <strong>Name:</strong>
        {{ boardingHouse.name }}
      </p>

      <p>
        <strong>Location:</strong>
        {{ boardingHouse.location }}
      </p>

      <p>
        <strong>Price:</strong>
        ₱{{ boardingHouse.price }}
      </p>

      <p>
        <strong>Room Type:</strong>
        {{ boardingHouse.roomType }}
      </p>

      <p>
        <strong>Availability:</strong>
        {{ boardingHouse.availability }}
      </p>

      <button @click="startEditing">
        Edit
      </button>

      <button @click="deleteBoardingHouse">
        Delete
      </button>

      <hr>

    </div>

    <div v-else>

      <input
        v-model="editedBoardingHouse.name"
        type="text"
        placeholder="Boarding house name"
      />

      <input
        v-model="editedBoardingHouse.location"
        type="text"
        placeholder="Location"
      />

      <input
        v-model="editedBoardingHouse.price"
        type="number"
        placeholder="Price"
      />

      <input
        v-model="editedBoardingHouse.roomType"
        type="text"
        placeholder="Room type"
      />

      <select v-model="editedBoardingHouse.availability">
        <option value="Available">Available</option>
        <option value="Full">Full</option>
      </select>

      <button @click="updateBoardingHouse">
        Save
      </button>

      <button @click="isEditing = false">
        Cancel
      </button>

    </div>

  </div>

</template>