<template>
  <div id="menu-overlay" class="menu-overlay">
    <div class="menu-overlay__title">
      <h1>Menu</h1>
    </div>
    <div class="menu-overlay__content">
      <button ref="start" @click="$root.start">Start</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MenuOverlay',
}
</script>

<style scoped>
.menu-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 3;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

h1 {
  color: #008000;
  text-transform: uppercase;
  font-size: 4rem;
  margin-bottom: 30px;
  display: none;
}

button {
  background: #008000;
  color: #fff;
  border: none;
  padding: 1rem 2rem;
  font-size: 1.5rem;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 10px;
}

button:hover {
  background: #00af00;
}

</style>
