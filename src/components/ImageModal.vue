<template>
  <div class="modal" v-if="isVisible" @click.self="closeModal">
    <div class="modal-content">
      <div class="image-container">
        <span class="close" @click="closeModal">&times;</span>
        <img :src="image.urls.regular" :alt="image.alt_description" width="50" height="10" class="modal-image" />
        <div class="overlay">
          <h4>{{ image.user.name }}</h4>
          <p>{{ image.user.location }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    image: Object, 
    isVisible: Boolean, 
  },
  mounted() {
 
    window.addEventListener('keydown', this.handleKeydown);
  },
  beforeUnmount() {

    window.removeEventListener('keydown', this.handleKeydown);
  },
  methods: {
    closeModal() {
      this.$emit('close'); 
    },
    handleKeydown(event) {
      if (event.key === 'Escape') {
        this.closeModal(); 
      }
    },
  },
};
</script>

<style scoped>
.modal {
  display: flex;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  justify-content: center;
  align-items: center;
}

.modal-content {
  position: relative;
  width: 500px;
  height: 400px; 
  max-width: 90%;
  background: transparent; 
  padding: 0; 
  border-radius: 10px;
}

.image-container {
  position: relative; 
  text-align: center; 
}

.modal-image {
  width: 100%; 
  height: 500px; 
  border-radius: 10px;
  margin-top: -20px; 
}

.overlay {
  position: absolute;
  bottom: 5px; 
  color: black; 
  background-color: #fff; 
  width:92%;
 text-align:left;
 padding:0px 20px;
  border-radius: 5px;
}

.close {
  position: absolute;
  top: 10px;
  right: 20px;
  font-size: 24px;
  cursor: pointer;
  color: white; 
  z-index: 10;
}
</style>
