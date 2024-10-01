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

     <div v-if="searchQuery.trim()" class="search-result-message">
     <p> Search results for "{{ searchQuery }}"</p>  
    </div> 
     
  </div>
 


   <div class="image-results">

      <template v-if="isLoading">
        <div v-for="n in 7" :key="n" class="placeholder-card">
          <!-- Placeholder skeleton loader here -->
          <div class="placeholder-image"></div>
          <div class="placeholder-details"></div>
        </div>
      </template>

      <template v-else>
        <ImageCard
          v-for="image in images"
          :key="image.id"
          :image="image"
          @click="openModal(image)"
        />
      </template>
   
    </div>

  <ImageModal
      :image="selectedImage"
      :isVisible="isModalVisible"
      @close="closeModal"
    />

   
</template>

<script>
import axios from 'axios';
import ImageCard from './components/ImageCard.vue';
import ImageModal from './components/ImageModal.vue';
export default {
  data() {
    return {
      searchQuery: '',
      images: [],
      selectedImage: null, 
      isModalVisible: false,
      isLoading: false,
    };
  },
    mounted() {   
 this.fetchImagesFromUnsplash('Africa');
  },
   components: {
    ImageCard, 
    ImageModal
  },
  methods: {
   handleInput() {      
      clearTimeout(this.timeout);    
      this.timeout = setTimeout(() => {
        this.searchImages();
      }, 1000);
    },
     searchImages() {
      if (this.searchQuery.trim() === '') {
        return; 
      }
      this.isLoading = true;
    
     setTimeout(() => {
      this.fetchImagesFromUnsplash(this.searchQuery);
     },1000)
    },

    fetchImagesFromUnsplash(query) {   
      
      const client_id = "f5IsOGO1gJrNGqGXLXb4AG5JKzD-PvEFVLYHob9IaAo";
      axios.get(`https://api.unsplash.com/search/photos?query=${query}&page=1&per_page=7&client_id=${client_id}`)
      .then((response) => {        
        this.images = response.data.results; 
        this.isLoading = false;
      })
      .catch(error => {
        console.error('Error fetching images:', error);
      })
      .finally(() => {
          this.isLoading = false; 
        });
    },
     openModal(image) {
      this.selectedImage = image; 
      this.isModalVisible = true; 
    },
    closeModal() {
      this.isModalVisible = false;
      this.selectedImage = null; 
    },
  },
};
</script>

<style lang="scss" scoped>

.search-result-message {
  text-align: center;
  margin: 20px;
  font-size: 18px;
  font-weight: bold;
}

.placeholder-card {
  width: 100%;
  max-width: 300px;
  margin: 15px;
  background-color: #f0f0f0;
  border-radius: 8px;
}

.placeholder-image {
  width: 100%;
  height: 200px;
  background-color: #e0e0e0;
  border-radius: 8px;
}

.placeholder-details {
  height: 20px;
  width: 80%;
  margin: 10px auto;
  background-color: #d0d0d0;
  border-radius: 4px;
}

.search-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
   background-color:#D3D1CB  ;
   padding:4.5rem 6rem;
}

.input-wrapper {
  position: relative;
  width: 100%;
  max-width: 500px;
 
}

.search-input {
  width: 100%;
  padding-left: 35px; 
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
  left: 10px; 
  top: 50%;
  transform: translateY(-50%);
  font-size: 16px;
  color: #999;
}


.image-results {
  display: grid;
  grid-template-columns: repeat(3, 1fr); 
  grid-gap: 20px;
  padding: 20px 100px 20px 100px;
  text-align:center;
  margin-top:-90px;
}

@media (max-width: 768px) {
  .image-results {
    grid-template-columns: 1fr; /* Single column on small screens */
    padding: 20px;
     margin-top:0px;
  }

  .search-container {
    padding: 2rem 1rem;
    width: 90%;
    
  }

  .search-input {
    font-size: 14px;
    padding-left: 30px;
    width: 80%;
  }
}

</style>
