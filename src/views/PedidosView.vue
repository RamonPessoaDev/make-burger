<template>
  <div class="main-container">
    <h1>Gerenciar Pedidos:</h1>
    <div class="dashboard-container">
      <Dashboard />
    </div>
  </div>
</template>

<script>
import Dashboard from "../components/Dashboard.vue";

export default {
  name: "Pedidos",
  components: {
    Dashboard
  }
};
</script>

<style scoped>
.main-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.dashboard-container {
  margin-top: 20px;
}
</style>
