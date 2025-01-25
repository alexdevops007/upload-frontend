<template>
  <div>
    <h2>Uploader un document</h2>
    <input type="file" @change="handleFileUpload" />
    <button @click="uploadFile">Uploader</button>
    <p v-if="error">{{ error }}</p>
    <p v-if="success">{{ success }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      file: null,
      error: '',
      success: '',
    };
  },
  methods: {
    handleFileUpload(event) {
      const selectedFile = event.target.files[0];
      if (selectedFile && selectedFile.size > 8 * 1024 * 1024) { // 8 Mo
        this.error = 'La taille du fichier ne doit pas dépasser 8 Mo.';
        this.file = null;
      } else {
        this.error = '';
        this.file = selectedFile;
      }
    },
    async uploadFile() {
      if (!this.file) {
        this.error = 'Veuillez sélectionner un fichier.';
        return;
      }

      const formData = new FormData();
      formData.append('file', this.file);

      try {
        await axios.post('https://upload-backend-teal.vercel.app/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        });
        this.success = 'Fichier uploadé avec succès!';
        this.file = null;
        this.$emit('fileUploaded');
      } catch (err) {
        if (err.response && err.response.data && err.response.data.message) {
          this.error = err.response.data.message;
        } else {
          this.error = 'Erreur lors de l\'upload du fichier.';
        }
      }
    },
  },
};
</script>
