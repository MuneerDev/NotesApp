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
  <ModelDelete
    v-if="isDelete"
    @close="closeModel"
    @delete="confirmDelete = true">
    ></ModelDelete
  >
  <div class="container mx-auto max-w-[90%]">
    <Addnote @add="addNoteToArray"></Addnote>
    <NotesList
      :Notes="NotesArray"
      @delet="DeleteNote"
      @edit="EditNote"></NotesList>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import Addnote from "./components/addNote";
import NotesList from "./components/NotesList";
import Model from "./components/ModelForm";
import ModelDelete from "./components/ModelDelete";

const isEditing = ref(false);
const isDelete = ref(false);
const confirmDelete = ref(null);

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
  isDelete.value = true;
  watch([isDelete, confirmDelete], ([isDeleteValue, confirmDeleteValue]) => {
    if (isDeleteValue && confirmDeleteValue) {
      NotesArray.value.splice(SelectedNote.index, 1);
      closeModel();
      confirmDelete.value = false;
      localStorage.setItem("NotesArray", JSON.stringify(NotesArray.value));
    }
    if (isDeleteValue && confirmDeleteValue === false) {
      confirmDelete.value = null;
    }
  });
};

const EditNote = (SelectedNote) => {
  isEditing.value = true;
  currentindex.value = SelectedNote.index;
};
const closeModel = () => {
  if (isEditing.value) {
    isEditing.value = false;
  } else {
    isDelete.value = false;
  }
};
const saveEdit = (SelectedEdit) => {
  NotesArray.value[SelectedEdit.index].messege = SelectedEdit.EditMessege;
  NotesArray.value[SelectedEdit.index].description = SelectedEdit.Editdesc;
  isEditing.value = false;
  localStorage.setItem("NotesArray", JSON.stringify(NotesArray.value));
};
</script>
