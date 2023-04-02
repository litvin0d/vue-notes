<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const error = ref(false);

const getRandomColor = () => {
    return `hsl(${Math.random() * 360}, 100%, 75%)`;
};

const addNote = () => {
    if (newNote.value.length > 10) {
        error.value = false;
        notes.value.push({
            id: Math.floor(Math.random() * 1000000),
            text: newNote.value,
            date: new Date(),
            bgColor: getRandomColor(),
        });
        showModal.value = false;
        newNote.value = "";
    } else {
        error.value = true;
    }
};

</script>

<template>
    <main>
        <div v-if="showModal" class="overlay">
            <div class="modal">
                <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
                <p v-if="error" class="error">Note must be at least 10 symbols</p>
                <button @click="addNote">Add Note</button>
                <button @click="showModal = false" class="close">Close</button>
            </div>
        </div>
        <div class="wrapper">
            <header>
                <h1>Notes</h1>
                <button @click="showModal = true">+</button>
            </header>
            <div class="cards-wrapper">
                <div v-for="note in notes" :key="note.id" class="card" :style="{background: note.bgColor}">
                    <p class="text">{{ note.text }}</p>
                    <p class="date">{{ note.date.toLocaleDateString("ru-RU") }}</p>
                </div>
            </div>
        </div>
    </main>
</template>

<style scoped>
main {
    width: 100vw;
    height: 100vh;
    background: var(--color-background-mute);
}

.overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background: #000000C4;
    z-index: 1;
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal {
    width: 750px;
    background: var(--color-background-mute);
    border-radius: 5px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
    transition: all .25s ease-in-out;
}

.error {
    color: #8B0000FF;
}

.modal > button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background: var(--vt-c-indigo);
    color: var(--vt-c-text-dark-1);
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 16px;
}

.modal > .close {
    background: #8B0000FF;
    margin-top: 8px;
}

.wrapper {
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
    font-weight: 600;
    font-size: 74px;
    margin-bottom: 25px;
}

header > button {
    border: none;
    width: 50px;
    height: 50px;
    padding: 10px;
    font-size: 20px;
    background: var(--vt-c-indigo);
    color: var(--vt-c-text-dark-1);
    border-radius: 100%;
    cursor: pointer;
}

.cards-wrapper {
    display: flex;
    flex-wrap: wrap;
}

.card {
    width: 225px;
    height: 225px;
    color: var(--vt-c-text-light-1);
    padding: 10px;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
}

.text {
    overflow-wrap: break-word;
}

.date {
    font-size: 12px;
    font-weight: 600;
}
</style>
