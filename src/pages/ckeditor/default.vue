<script setup>
import { ref } from "vue";
import { useMeta } from "meta";
import ClassicEditor from "@/features/ckeditor/model/build.js";
import "@/features/ckeditor/ui/base.scss";

useMeta({
  title: "editor : ckeditor - default",
  description: "ckeditor normal editor",
});

const editor = ref(ClassicEditor);
const instance = ref(null);
const content = ref(null);
const fileInput = ref(null);

const ready = (edit) => instance.value = edit;
const getData = async () => alert(content.value);
const setData = (html) => content.value = html;

const onUpload = async () => fileInput.value.click();

async function onChange(event) {
  const files = Array.from(event.target.files);
  const file = files[0];
  if (file.type !== "text/html") return;

  const reader = new FileReader();
  reader.onload = (e) => setData(e.target.result);
  reader.readAsText(file);
  event.target.value = "";
}
</script>

<template>
  <v-container>
    <h1>CKEditor</h1>
    <span>default normal editor</span>
    <div>
      <v-btn @click="getData">Get Data</v-btn>
      <v-btn @click="onUpload">File upload</v-btn>
      <v-file-input hide-input prepend-icon="" ref="fileInput" @change="onChange" />
    </div>
    <div class="ck-container">
      <ckeditor
        v-model="content"
        :editor="editor"
        @ready="ready"
      />
    </div>
  </v-container>
</template>

<style lang="scss">
.v-container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  gap: 10px;
}
</style>
