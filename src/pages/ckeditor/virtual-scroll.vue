<script setup>
import { ref } from "vue";
import { useMeta } from "meta";
import ClassicEditor from "@/features/ckeditor/model/build.js";
import $plugin from "@/features/ckeditor/api/virtual-scroll/index.js"
import "@/features/ckeditor/ui/global.scss";

useMeta({
  title: "editor : ckeditor - Virtual Scrolling",
  description: "ckeditor Virtual Scrolling",
});

const editor = ref(ClassicEditor);
const fileInput = ref(null);

function ready(instance) {
  $plugin.init(instance);
}

async function getData() {
  alert(await $plugin.getData());
}

function setData(html) {
  $plugin.setData(html);
}

async function onUpload() {
  if (fileInput.value) {
    fileInput.value.click();
  }
}

async function onChange(event) {
  const files = Array.from(event.target.files);
  const file = files[0];
  if (file.type !== "text/html") return;

  const reader = new FileReader();
  reader.onload = (e) => {
    const content = e.target.result;
    console.log(content);
    setData(content);
  };
  reader.readAsText(file);
}
</script>

<template>
  <v-container>
    <h1>CKEditor</h1>
    <span>Virtual Scroll</span>
    <div>
      <v-btn @click="getData">Get Data</v-btn>
      <v-btn @click="onUpload">File upload</v-btn>
      <v-file-input hide-input prepend-icon="" ref="fileInput" @change="onChange" />
    </div>
    <div class="ck-container">
      <ckeditor :editor @ready="ready" />
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
