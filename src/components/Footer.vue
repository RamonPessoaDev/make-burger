<template>
  <footer id="footer">
    <p>Make Your Burger &copy; 2024</p>
  </footer>
</template>

<script>
export default {
  name: "Footer"
};
</script>

<style scoped>
#footer {
  height: 200px;
  background-color: #222;
  border-top: 4px solid #111;
  color: #fcba03;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
}

@media (max-width: 375px) {
  #footer {
    width: 360px;
  }
}

@media (max-width: 299px) {
  #footer {
    width: 285px;
  }
}
</style>
