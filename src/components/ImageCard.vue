
<template>
  <div class="image-card">
    <img :src="image.urls.small" :alt="image.alt_description" />
    <div class="image-details">
      <h4>{{ image.user.name }}</h4>
      <p>{{ image.user.location }}</p>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    image: Object,  
  },
    mounted() {
  },
};


</script>

<style lang="scss" scoped>
.image-card {
  position: relative;
  overflow: hidden;
  border-radius: 10px;
  transition: transform 0.3s ease;
  cursor: pointer;
  padding-top:20px;
  
  
  &:nth-child(odd) {
    transform: translateY(-10px); 
  }
  
  &:nth-child(even) {
    transform: translateY(10px); 
  }

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 10px;
  }

  .image-details {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    padding: 10px;
    background: rgba(0, 0, 0, 0.5); 
    color: white;
    text-align: left;
    transition: background 0.3s ease;  
    
    h4 {
      margin: 0;
      font-size: 16px;
      white-space: nowrap; 
      overflow: hidden; 
      text-overflow: ellipsis;
    }

    p {
      margin: 0;
      font-size: 14px;
      white-space: nowrap; 
      overflow: hidden;
      text-overflow: ellipsis; 
    }
  }

  &:hover {
    transform: scale(1.05); 
  }

  &:hover .image-details {
    background: rgba(0, 0, 0, 0.7); 
  }
}

</style>
