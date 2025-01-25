<template>
  <div class="upload">
    <h2>Télécharger un Document</h2>
    <form @submit.prevent="handleUpload">
      <input type="file" @change="onFileChange" />
      <button type="submit">Télécharger</button>
    </form>
    <p v-if="message" :class="{'error': isError, 'success': !isError}">
      {{ message }}
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      file: null,
      message: '',
      isError: false,
    };
  },
  methods: {
    onFileChange(event) {
      const selectedFile = event.target.files[0];
      if (selectedFile && selectedFile.size > 8 * 1024 * 1024) {
        this.message = 'La taille du fichier dépasse 8MB.';
        this.isError = true;
        this.file = null;
      } else {
        this.file = selectedFile;
        this.message = '';
      }
    },
    async handleUpload() {
      if (!this.file) {
        this.message = 'Veuillez sélectionner un fichier.';
        this.isError = true;
        return;
      }

      const formData = new FormData();
      formData.append('file', this.file);

      try {
        const response = await fetch(`upload-backend-2unt.onrender.com/api/documents/upload`, {
          method: 'POST',
          body: formData,
        });

        const data = await response.json();

        if (response.ok) {
          this.message = data.message;
          this.isError = false;
          this.file = null;
        } else {
          this.message = data.message || 'Erreur lors du téléchargement.';
          this.isError = true;
        }
      } catch (error) {
        this.message = 'Erreur de connexion au serveur.';
        this.isError = true;
      }
    },
  },
};
</script>

<style scoped>
.error {
  color: red;
}
.success {
  color: green;
}
</style>
