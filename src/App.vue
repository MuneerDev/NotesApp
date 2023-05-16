<template>
  <Model
    v-if="isEditing"
    :isEditing="isEditing"
    @close="closeModel"
    @save="saveEdit"
    :currentindex="currentindex"
    :NotesArray="NotesArray">
    <button
      class="bg-red-500 px-3 py-2 rounded-xl text-white"
      @click="closeModel">
      cancel
    </button>
  </Model>
  <div class="container mx-auto max-w-[90%]">
    <Addnote @add="addNoteToArray"></Addnote>
    <NotesList
      :Notes="NotesArray"
      @delet="DeleteNote"
      @edit="EditNote"></NotesList>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import Addnote from "./components/addNote";
import NotesList from "./components/NotesList";
import Model from "./components/ModelForm";

const isEditing = ref(false);

const currentindex = ref(null);

const NotesArray = ref([]);
const addNoteToArray = (note) => {
  NotesArray.value.push(note);
  localStorage.setItem("NotesArray", JSON.stringify(NotesArray.value));
};

onMounted(() => {
  if (localStorage.getItem("NotesArray")) {
    const Notes = JSON.parse(localStorage.getItem("NotesArray"));
    NotesArray.value = Notes;
  }
});

const DeleteNote = (SelectedNote) => {
  NotesArray.value.splice(SelectedNote.index, 1);
  localStorage.setItem("NotesArray", JSON.stringify(NotesArray.value));
};
const EditNote = (SelectedNote) => {
  isEditing.value = true;
  currentindex.value = SelectedNote.index;
};
const closeModel = () => {
  isEditing.value = false;
};
const saveEdit = (SelectedEdit) => {
  NotesArray.value[SelectedEdit.index].messege = SelectedEdit.EditMessege;
  NotesArray.value[SelectedEdit.index].description = SelectedEdit.Editdesc;
  isEditing.value = false;
  localStorage.setItem("NotesArray", JSON.stringify(NotesArray.value));
};
</script>
