<script setup>
import { ref } from 'vue';

const showModal = ref(false)
const newNote = ref('')
const errMsg = ref('')
const noteObj = ref([])

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}
const addNote = () => {
  if(newNote.value.length < 6) {
    return errMsg.value = 'Note requires at least 5 words to add!'
  }
  noteObj.value.push({
    id: noteObj.value.length + 1,
    note: newNote.value,
    date: new Date(),
    color: getRandomColor()
  })
  // clear textarea
  newNote.value = ''
  // toggle modal
  showModal.value = !showModal.value
  // clear errMsg
  errMsg.value = ''
}
const handleDelete = (id) =>{
  const afterDeleted = noteObj.value.filter(note => note.id !== id)
  noteObj.value = afterDeleted

}

</script>
<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errMsg">{{ errMsg }}</p>
        <button @click="addNote">Add Notes</button>
        <button @click="showModal = !showModal" class="close">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = !showModal">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in noteObj" :key="note.id" class="card" :style="{ background: note.color }">
          <p class="card-text">{{ note.note }}</p>
          <div class="card-footer">
            <p class="card-date">{{ note.date.toLocaleDateString('en-US') }}</p>
            <button @click="handleDelete(note.id)">Delete</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  font-size: 75px;
}

header button {
  border: none;
  background: rgb(21, 20, 20);
  color: white;
  width: 50px;
  height: 50px;
  font-size: 30px;
  border-radius: 100px;
  cursor: pointer;
}

.card {
  width: 225px;
  height: 225px;
  background: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.card button {
  padding: 10px;
  color: white;
  font-size: 15px;
  border-radius: 5px;
  background: rgb(220, 62, 62);
  border: none;
  cursor: pointer;
}

.card-date {
  font-size: 12.5px;
  font-weight: bold;
}

.card-text {
  font-size: 25px;
}
.card-footer{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  width: 750px;
  background: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px;
  width: 100%;
  font-size: 20px;
  margin-top: 15px;
  border: none;
  color: white;
  background: blueviolet;
  cursor: pointer;
}

.modal .close {
  background: rgb(235, 10, 10);
}
.modal p{
  color:rgb(235, 10, 10)
}
</style>