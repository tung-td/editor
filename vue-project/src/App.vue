<script>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faFile } from '@fortawesome/free-solid-svg-icons'

export default {
  components: {
    FontAwesomeIcon
  },
  data() {
    return {
      content: '',
      fileName: ''
    }
  },
  methods: {
    updateContent(event) {
      this.content = event.target.innerHTML;
    },
    loadFile(event) {
      const file = event.target.files[0];
      this.fileName = file.name;
      const reader = new FileReader();
      reader.onload = () => {
        this.content = reader.result;
      };
      reader.readAsText(file);
    },
    saveFile() {
      const blob = new Blob([this.content], { type: 'text/plain' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'file.txt';
      document.body.appendChild(a);
      a.click();
      window.URL.revokeObjectURL(url);
      document.body.removeChild(a);
    },
    openFile() {
        this.$refs.fileInput.click();
      }
  }
}
</script>

<template>
  <div class="container">
    <div class="sidebar">
      <h4>Editor Code App</h4>
      <div class="groupd-btn">
        <button class="btn" @click="openFile">Open</button>
        <button class="btn" @click="saveFile">Save</button>
        <button class="btn" @click="saveFile">Clear</button>
      </div>
    </div>
    <div class="content">
      <div class="files">
        <div class="file"><i class="fas fa-file"></i>{{ fileName }}</div>
      </div>
      <div id="editor" contenteditable="true" v-html="content" @input="updateContent"></div>
      <input type="file" ref="fileInput" style="display: none;" @change="loadFile">
    </div>
  </div>
</template>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

#editor {
  width: 100%;
  height: 100%;
  resize: vertical; 
  overflow: auto;
}
.container {
  display: flex;
  height: 100%;
}
.sidebar {
  background-color: #545454;
  width: 15%;
  padding: 10px;
}
.sidebar > h4 {
  color: #fff;
}
.btn {
  background-color: transparent !important;
    border: unset !important;
    color: #fff !important;
    font-weight: bold !important;
    margin: 0 10px !important;
}
.content {
  width: 85%;
}
.files {
  background-color: #191919;
  height: 30px;
  width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
}
.file {
  color: #fff;
  font-weight: bold;
  margin: 0px 10px;
}
</style>
