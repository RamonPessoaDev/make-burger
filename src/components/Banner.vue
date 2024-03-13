<template>
  <div id="main-banner">
    <h1>Make Your Burger</h1>
  </div>
</template>

<script>
export default {
  name: "Banner"
};
</script>

<style scoped>
#main-banner {
  background-image: url("../../public/img/burger.jpg");
  background-position: 0 -410px;
  background-size: cover;
  height: 600px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

#main-banner h1 {
  color: #fff;
  text-align: center;
  font-size: 60px;
  background-color: #222;
  padding: 20px 40px;
}

@media (max-width: 1650px) {
  #main-banner {
    background-position: 0 -310px;
  }
}

@media (max-width: 1024px) {
  #main-banner {
    height: 360px;
    background-position: 0 -200px;
  }

  #main-banner h1 {
    text-align: start;
    font-size: 30px;
    padding: 10px 20px;
  }
}

@media (max-width: 600px) {
  #main-banner {
    height: 320px;
    background-position: 0 -320px;
    background-position-x: -30px;
  }

  #main-banner h1 {
    text-align: center;
    font-size: 16px;
    padding: 10px 20px;
  }
}

@media (max-width: 375px) {
  #main-banner {
    background-position-x: -70px;
    width: 360px;
  }

  #main-banner h1 {
    font-size: 12px;
    padding: 8px 15px;
  }
}

@media (max-width: 299px) {
  #main-banner {
    background-position-x: -130px;
    width: 285px;
  }

  #main-banner h1 {
    font-size: 10px;
    padding: 8px 15px;
  }
}
</style>
