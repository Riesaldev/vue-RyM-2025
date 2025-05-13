<script setup>
import { ref, onMounted, computed } from 'vue';

const searchQuery = ref('');

const characters = ref([]);

const filteredCharacters = computed(() => {
  return characters.value.filter(character => 
    character.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

const fetchCharacters = async () => {
  try {
    const response = await fetch(`https://rickandmortyapi.com/api/character`);
    const data = await response.json();
    characters.value = data.results;
  } catch (error) {
    console.error('Error fetching characters:', error);
  }
};

onMounted(() => {
  fetchCharacters();
});
</script>

<template>
  <nav class="navbar">
    <div class="navbar-content">
      <h1 class="navbar-title">Rick and Morty Gallery</h1>
      <div class="search-container">
        <input 
          type="text"
          placeholder="Buscar..."
          class="search-input"
          v-model="searchQuery"
        />
      </div>
    </div>
  </nav>
  <main>
    <div class="character-grid">
      <div class="character-card" v-for="character in filteredCharacters" :key="character.id">
        <img :src="character.image" :alt="character.name" />
        <h2>{{ character.name }}</h2>
        <p>{{ character.species }}</p>
      </div>
    </div>
  </main>
</template>

<style scoped>
body {
  margin: 0;
  background: #181818;
}

.navbar {
  background-color: #333;
  color: #fff;
  padding: 10px 0;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  box-sizing: border-box;
  z-index: 1000;
}

.navbar-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  max-width: 100vw;
  padding: 0 32px;
  box-sizing: border-box;
}

.navbar-title {
  font-size: 24px;
  margin: 0;
}

.search-container {
  display: flex;
  align-items: center;
}

.search-input {
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  width: 200px;
}

main {
  margin-top: 60px;
}

.character-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
  padding: 32px;
  width: 100%;
  box-sizing: border-box;
}

.character-card {
  border-radius: 8px;
  padding: 8px 0 0 0;
  text-align: center;
  background: none;
  box-shadow: none;
  color: #eee;
}

.character-card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: 6px;
  margin-bottom: 10px;
  background: #222;
}

.character-card h2 {
  margin: 10px 0 4px 0;
  font-size: 1.1em;
  color: #eee;
}

.character-card p {
  color: #bdbdbd;
  margin: 5px 0 10px 0;
}
</style>
