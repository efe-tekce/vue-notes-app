<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);
const addNote = () => {
    if (newNote.value.length < 5) {
        return (errorMessage.value =
            "A note needs to be minimum 5 characters.");
    }
    notes.value.push({
        id: Math.floor(Math.random() * 1000000),
        text: newNote.value,
        date: new Date(),
        backgroundColor: getRandomColor(),
    });
    showModal.value = false;
    newNote.value = "";
};
function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}
</script>

<template>
    <main>
        <div class="overlay" v-if="showModal">
            <div class="modal">
                <textarea
                    v-model.trim="newNote"
                    name="note"
                    id="note"
                    cols="30"
                    rows="10"
                ></textarea>
                <p v-if="errorMessage">{{ errorMessage }}</p>
                <button @click="addNote">Add Note</button>
                <button class="close" @click="showModal = false">Close</button>
            </div>
        </div>
        <div class="container">
            <header>
                <h1>Notes</h1>
                <button @click="showModal = true">+</button>
            </header>
            <div class="cards-container">
                <div
                    class="card"
                    v-for="note in notes"
                    :key="note.id"
                    :style="{ backgroundColor: note.backgroundColor }"
                >
                    <p class="main-text">
                        {{ note.text }}
                    </p>
                    <p class="date">
                        {{ note.date.toLocaleDateString("en-US") }}
                    </p>
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
    margin-bottom: 25px;
    font-size: 75px;
}
button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(21, 20, 20);
    border-radius: 100%;
    color: white;
    font-size: 20px;
}
.card {
    width: 225px;
    height: 225px;
    background: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
}
.date {
    font-size: 13px;
    font-weight: bold;
}
.cards-container {
    display: flex;
    flex-wrap: wrap;
}
.overlay {
    position: absolute;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
}
.modal {
    width: 750px;
    background-color: cyan;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.modal button {
    padding: 10px;
    font-size: 20px;
    width: 50%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
}
.modal .close {
    background-color: darkred;
    margin-top: 7px;
}
.modal p {
    color: darkred;
}

textarea {
    max-width: 50%;
    max-height: 50%;
}
</style>
