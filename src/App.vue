<template>
    <main>
        <div class="overlay" v-if="showModal">
            <div class="modal">
                <textarea
                    v-model.trim="newNote"
                    name="notes"
                    id="notes"
                    cols="30"
                    rows="10"
                ></textarea>
                <p v-if="errorMessage">{{ errorMessage }}</p>
                <button @click="addNote">Add Note</button>
                <button class="close-button" @click="showModal = false">
                    Close
                </button>
            </div>
        </div>
        <div class="container">
            <header>
                <h1>Notes</h1>
                <button @click="openModal">+</button>
            </header>
            <h3 v-if="notes.length === 0" class="no-notes-message">
                Tap "+" to add notes
            </h3>
            <div class="cards-container">
                <div v-for="note in notes" :key="note.id" class="card">
                    <div class="edit-note">
                        <span
                            @click="toggleDropdown(note.id)"
                            class="material-symbols-outlined icon"
                        >
                            more_horiz
                        </span>
                    </div>
                    <div v-if="dropdownVisibility === note.id" class="dropdown">
                        <p @click="editNote(note)">Edit</p>
                        <p @click="deleteNote(note.id)">Delete</p>
                    </div>
                    <p class="note-text">
                        {{ note.text }}
                    </p>
                    <p class="note-date">
                        {{ note.date.toLocaleDateString("en-US") }}
                    </p>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMessage = ref("");
const dropdownVisibility = ref(null);

const openModal = () => {
    newNote.value = "";
    errorMessage.value = "";
    showModal.value = true;
};

const addNote = () => {
    if (newNote.value.length < 10) {
        return (errorMessage.value =
            "Note must be at least 10 characters long");
    }
    notes.value.push({
        text: newNote.value,
        date: new Date(),
        id: Math.floor(Math.random() * 1000000),
    });
    showModal.value = false;
    newNote.value = "";
    errorMessage.value = "";
};

const toggleDropdown = (noteId) => {
    if (dropdownVisibility.value === noteId) {
        dropdownVisibility.value = null;
    } else {
        dropdownVisibility.value = noteId;
    }
};

const editNote = (note) => {
    newNote.value = note.text;
    showModal.value = true;
    deleteNote(note.id);
};

const deleteNote = (noteId) => {
    notes.value = notes.value.filter((note) => note.id !== noteId);
};
</script>

<style scoped>
main {
    height: 100vh;
    width: 100vw;
}
.container {
    max-width: 1000px;
    padding: 25px;
    margin: 0 auto;
}
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
h1 {
    font-weight: bold;
    font-size: 40px;
}
header button {
    border: none;
    border-radius: 100%;
    padding: 10px 20px;
    cursor: pointer;
    height: 50px;
    width: 50px;
    font-size: 20px;
    font-weight: bold;
    background-color: #f5f5f5;
    color: #3c3d37;
    transition: all 0.3s;
}
header button:hover {
    background-color: blueviolet;
}
.no-notes-message {
    font-style: italic;
    color: #777;
    text-align: center;
    margin-top: 100px;
}
.cards-container {
    display: flex;
    flex-wrap: wrap;
}
.card {
    width: 225px;
    height: 225px;
    margin: 30px 20px 30px 0;
    background-color: #f5f5f5;
    padding: 10px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    position: relative;
}
.edit-note {
    position: absolute;
    right: 8px;
    top: 8px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #3c3d37;
    border-radius: 100%;
    width: 30px;
    height: 30px;
}
.icon {
    color: #f5f5f5;
    cursor: pointer;
}
.dropdown {
    position: absolute;
    top: 40px;
    right: 0;
    background-color: #f5f5f5;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    padding: 10px;
    z-index: 100;
    width: 100px;
}
.dropdown p {
    margin: 0;
    padding: 5px;
    cursor: pointer;
    font-size: 14px;
    color: #333;
    transition: background-color 0.2s;
}
.dropdown p:hover {
    background-color: #e5e5e5;
}
.note-text {
    color: #3c3d37;
    margin-right: 30px;
    word-wrap: break-word;
    overflow: hidden;
    text-overflow: ellipsis;
}
.note-date {
    color: #3c3d37;
    font-size: 12px;
    font-weight: bold;
}
.overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: 10;
    background-color: rgb(30, 32, 30, 0.8);
    display: flex;
    align-items: center;
    justify-content: center;
}
.modal {
    width: 600px;
    background-color: #f5f5f5;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
}
.modal p {
    color: red;
}
.modal button {
    padding: 10px 20px;
    cursor: pointer;
    border: none;
    border-radius: 10px;
    margin-top: 8px;
    background-color: blueviolet;
    color: #f5f5f5;
    font-size: 17px;
    font-weight: 500;
    transition: all 0.3s;
}
.modal .close-button {
    background-color: #ff6600;
}
.modal button:hover {
    opacity: 0.85;
}
</style>
