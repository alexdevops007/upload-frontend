<template>
  <div class="document-list">
    <h2>Documents Uploadés</h2>
    <ul>
      <li v-for="doc in documents" :key="doc._id">
        <a
          :href="`https://upload-backend-2unt.onrender.com/api/documents/${doc._id}`"
          target="_blank"
        >
          {{ doc.originalName }} ({{ formatSize(doc.size) }})
        </a>
        <span> - Téléchargé le {{ formatDate(doc.uploadDate) }}</span>
      </li>
    </ul>
    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      documents: [],
      error: "",
    };
  },
  methods: {
    async fetchDocuments() {
      try {
        const response = await fetch(
          `https://upload-backend-2unt.onrender.com/api/documents`
        );
        const data = await response.json();
        if (response.ok) {
          this.documents = data;
        } else {
          this.error =
            data.message || "Erreur lors de la récupération des documents.";
        }
      } catch (err) {
        this.error = "Erreur de connexion au serveur.";
      }
    },
    formatSize(size) {
      if (size < 1024) return `${size} bytes`;
      else if (size < 1024 * 1024) return `${(size / 1024).toFixed(2)} KB`;
      else return `${(size / (1024 * 1024)).toFixed(2)} MB`;
    },
    formatDate(date) {
      return new Date(date).toLocaleDateString();
    },
  },
  created() {
    this.fetchDocuments();
  },
};
</script>

<style scoped>
.error {
  color: red;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 10px;
}
</style>
