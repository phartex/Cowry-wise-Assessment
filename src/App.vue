<template>
  <div class="search-container">
    <div class="input-wrapper">
      <i class="fas fa-search search-icon"></i>
      <input
        type="text"
        v-model="searchQuery"
        placeholder="Search for photo"
        class="search-input"
        @input="handleInput"
        @keydown.enter="searchImages"
      />
    </div>
    <div class="image-results">
      <!-- Render image results here -->
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      searchQuery: '',
    };
  },
  methods: {
   handleInput() {
      // Clear previous timeout
      clearTimeout(this.timeout);
      
      // Set a new timeout for 1 second
      this.timeout = setTimeout(() => {
        this.searchImages();
      }, 1000);
    },
     searchImages() {
      if (this.searchQuery.trim() === '') {
        return; // Don't call the API if the search query is empty
      }
      console.log('Searching for images:', this.searchQuery);
    this.fetchImagesFromUnsplash(this.searchQuery);
    },

      fetchImagesFromUnsplash(query) {   
         const client_id = "f5IsOGO1gJrNGqGXLXb4AG5JKzD-PvEFVLYHob9IaAo";
      axios.get(`https://api.unsplash.com/search/photos?query=${query}&page=1&per_page=7&client_id=${client_id}`)
      .then(response => {
        console.log(response.data.results);
      })
      .catch(error => {
        console.error('Error fetching images:', error);
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.search-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.input-wrapper {
  position: relative;
  width: 100%;
  max-width: 500px;
}

.search-input {
  width: 100%;
  padding-left: 35px; // Padding adjusted to leave space for the icon
  padding-right: 15px;
  padding-top: 10px;
  padding-bottom: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  outline: none;
}

.search-icon {
  position: absolute;
  left: 10px; // Positioned inside the input box on the left
  top: 50%;
  transform: translateY(-50%);
  font-size: 16px;
  color: #999;
}
</style>
