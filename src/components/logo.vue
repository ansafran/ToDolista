<template>
  <div class="logo-c">
    <img :src="src" alt="Logo" class="logo" />
  </div>
</template>

<script>
export default {
  name: 'AppLogo',  
  props: {
    src: {
      type: String,
      required: true 
    }
  }
};
</script>

<style scoped>
.logo-c {
  margin-bottom: 20px;
  justify-content: center;
  position: left;
}

.logo {
  width: 50px; 
  height: auto;
  margin-right: 15px;
}
</style>
