<template>
    <h1>Rooms</h1>
    <h2>Lorem ipsum dolor sit amet consectetur adipisicing elit. Asperiores dolore alias libero doloremque repellat perspiciatis iure nisi sequi.</h2>
    <div class="container">
      <div class="card" v-for="room in rooms">
        <img :src="room.thumbnail" :alt=room.name>
        <p class="title">Lorem Ipsum</p>
        <button class="button" v-if="room.spots != 0" @click="bookRoom(room)">Book!</button>
        <button class="button" v-else disabled>Fully Booked!</button>
        <p class="seats">{{ room.spots }} spots remaining</p>
      </div>
    </div>
</template>

<script setup lang="ts">import { onBeforeMount, ref } from 'vue';

type Room = {
  name: string,
  spots: number,
  thumbnail: string
}

const rooms = ref<Room[]>([]);

onBeforeMount( async () => {
  await getData();
})

const getData = async () => {

  await fetch('https://wetransfer.github.io/rooms.json')
      .then(response => response.json())
      .then(data => {
        rooms.value = data.rooms;
      }).catch(
        error => { console.error(error); }
      );
}

const bookRoom = (roomToBook: Room) => { 

  const room = rooms.value.find(room => room.name === roomToBook.name);
  if (room) {
    if (room.spots > 0)
      room.spots--;
  }
}
</script>

<style scoped>

h2 {
  width: 40%;
  color: rgb(191, 191, 191);
}

.container {
  display: flex;
  flex-wrap: wrap;
  align-items: stretch;
}

.card {
  flex: 0 500px;
  margin: 10px;
  border-radius: 10px;  
}

.title {
  font-weight: bold;
  display: inline-block;
} 

.seats {
  color: #940E7A;
}

.card img {
  max-width: 100%;
  border-radius: 10px;
}

.button {
  margin-top: 10px;
  float: right;
}

:disabled {
  opacity: 0.5;
}
</style>
