<template>
  <div class="home">
    <h1>Demande de Permis d'Exploitation</h1>
    <nav>
      <router-link to="/upload">Télécharger un Document</router-link>
      |
      <router-link to="/documents">Voir les Documents</router-link>
    </nav>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
};
</script>

<style scoped>
nav {
  margin-top: 20px;
}
a {
  margin: 0 10px;
}
</style>
