<template>
  <Banner />
  <div class="main-container">
    <h1>Monte o seu burger:</h1>
    <BurgerForm />
  </div>
</template>

<script>
import Banner from "../components/Banner.vue";
import BurgerForm from "../components/BurgerForm.vue";

export default {
  name: "Home",
  components: {
    Banner,
    BurgerForm
  }
};
</script>

<style scoped>
.main-container h1 {
  display: flex;
  max-width: 100%;
  text-align: center;
  justify-content: center;
  align-items: center;
}

@media (max-width: 1024px) {
  .main-container h1 {
    font-size: 32px;
    margin-left: 4px;
  }
}

@media (max-width: 600px) {
  .main-container h1 {
    display: inline-block;
    white-space: nowrap;
    font-size: 31.5px;
    margin-left: 0px;
  }
}

@media (max-width: 425px) {
  .main-container h1 {
    text-align: start;
    justify-content: flex-start;
    align-items: center;
    margin-left: 4px;
  }
}

@media (max-width: 414px) {
  .main-container h1 {
    margin-left: 0px;
  }
}

@media (max-width: 375px) {
  .main-container h1 {
    display: flex;
    text-align: center;
    justify-content: flex-start;
    align-items: center;
    font-size: 21px;
    margin-left: 0px;
  }
}

@media (max-width: 299px) {
  .main-container h1 {
    display: flex;
    text-align: center;
    justify-content: center;
    align-items: center;
    font-size: 21px;
    margin-left: 8px;
  }
}
</style>
