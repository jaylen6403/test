<template>
  
    <div class="father">
      <slot></slot>
    </div>
   
</template>




<script>
export default { 
  name : 'VnAlert',
  
};
</script>

<style scoped>

.father {
  display: flex;
  justify-content: center; 
  align-items: center;
  color: white;
  background-color:#2556c0;
  padding: 1%; 
  margin: -10px ;
  
}
</style>

