<template>
  <div>
    <h2>Liste des documents</h2>
    <ul>
      <li v-for="file in files" :key="file._id">
        <a :href="`https://upload-backend-teal.vercel.app/files/${file.filename}`" target="_blank">
          {{ file.filename }}
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      files: [],
    };
  },
  methods: {
    async fetchFiles() {
      try {
        const response = await axios.get('https://upload-backend-teal.vercel.app/files');
        this.files = response.data;
      } catch (err) {
        console.error('Erreur lors de la récupération des fichiers:', err);
      }
    },
  },
  created() {
    this.fetchFiles();
  },
};
</script>
