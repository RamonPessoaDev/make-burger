<template>
  <div id="burger-table">
    <div class="msg-container">
      <Message :msg="msg" v-show="msg" />
    </div>
    <div class="burger-table-container">
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div class="client-name">{{ burger.nome }}</div>
        <div class="bread-name">{{ burger.pao }}</div>
        <div>{{ burger.carne }}</div>
        <div class="opcionais-container">
          <ul v-for="(opcional, index) in burger.opcionais" :key="index">
            <li class="opcionais-li">
              {{ opcional }}
            </li>
          </ul>
        </div>
        <div>
          <select
            name="status"
            class="status"
            @change="updateBurger($event, burger.id)"
          >
            <option value="" selected disabled>Selecione</option>
            <option
              v-for="s in status"
              :key="s.id"
              :value="s.tipo"
              :selected="burger.status === s.tipo"
            >
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteBurger(burger.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

const URL = "http://localhost:3002";

export default {
  name: "Dashboard",
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      msg: null
    };
  },
  methods: {
    async getOrders() {
      const req = await fetch(
        // "https://makeburger-api.onrender.com/burgers"
        `${URL}/burgers`
      );

      const data = await req.json();

      this.burgers = data;

      //Resgata os status de pedidos
      this.getStatus();
    },
    async getStatus() {
      const req = await fetch(
        // "https://makeburger-api.onrender.com/status"
        `${URL}/status`
      );

      const data = await req.json();

      this.status = data;
    },
    async deleteBurger(id) {
      const req = await fetch(
        // `https://makeburger-api.onrender.com/burgers/${id}`
        `${URL}/burgers/${id}`,
        {
          method: "DELETE"
        }
      );

      const res = await req.json();

      this.msg = `Pedido Nº <strong>${res.id}</strong> removido com sucesso!`;

      //Limpar msg depois de 3seg
      setTimeout(() => (this.msg = ""), 3000);

      //Força uma atualização do sistema por meio do backend
      this.getOrders();
    },
    async updateBurger(event, id) {
      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option });

      const req = await fetch(
        // `https://makeburger-api.onrender.com/burgers/${id}`
        `${URL}/burgers/${id}`,
        {
          //Como se fosse o UPDATE, mas atualiza apenas o que enviamos, que nesse caso é o status, as outras informações não são alteradas
          method: "PATCH",
          headers: { "Content-Type": "application/json" },
          body: dataJson
        }
      );

      const res = await req.json();

      this.msg = `O pedido Nº <strong>${res.id}</strong> foi atualizado para: <strong>${res.status}</strong>`;

      //Limpar msg depois de 3seg
      setTimeout(() => (this.msg = ""), 3000);

      this.$nextTick(() => {
        const msgContainer = document.querySelector(".msg-container");
        if (msgContainer) {
          // Calcula a posição do topo do elemento .msg-container
          const topPosition = msgContainer.offsetTop;
          // msgContainer.getBoundingClientRect().top + window.scrollY;
          // Usa window.scrollTo para rolar a página até a posição calculada
          window.scrollTo({ top: topPosition, behavior: "smooth" });
        }
      });
    }
  },
  mounted() {
    this.getOrders();
  },
  components: {
    Message
  }
};
</script>

<style scoped>
#burger-table {
  max-width: 1200px;
  margin: 0 auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}

#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

.burger-table-row {
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

#burger-table-heading div,
.burger-table-row div {
  width: 18%;
  display: flex;
}

.burger-table-row .opcionais-li {
  margin-left: 16px;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 10%;
}

#burger-table-heading,
.burger-table-row {
  width: 100%;
  align-items: center;
}

.burger-table-row .opcionais-container {
  display: flex;
  flex-wrap: wrap;
  width: 10%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
  margin-left: 45%;
}

.status {
  cursor: pointer;
}

.status option {
  cursor: pointer;
}

.delete-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px 12px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: #5c5b5b;
  color: #222;
}

.message {
  text-align: center;
}

/* Media query para telas médias */
@media (max-width: 1024px) {
  #burger-table-rows {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .burger-table-row .opcionais-container {
    display: flex;
    flex-wrap: wrap;
    width: 5%;
  }
}

/* Media query para telas menores */
@media (max-width: 600px) {
  #burger-table {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  #burger-table-heading,
  .burger-table-row {
    width: 75%;
    align-items: center;
    display: flex;
  }

  #burger-table-rows,
  .burger-table-container {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  #burger-table-heading div,
  .burger-table-row div {
    width: 100%;
    display: flex;
    flex-direction: row;
    padding: 4px 0px;
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 100%;
  }
  .burger-table-row .opcionais-container {
    width: 45%;
    margin-right: 150px;
  }

  select {
    margin-left: 0;
    width: 100%;
  }

  .status {
    width: 120px;
  }

  .burger-table-row .opcionais-li {
    width: 90px;
  }

  .delete-btn {
    padding: 5px 8px;
  }
}

/* Ajustes para telas menores */
@media (max-width: 375px) {
  #burger-table {
    max-width: 900px; /* Limita a largura máxima para 900px */
    display: flex;
    justify-content: center; /* Alinha o conteúdo horizontalmente no centro */
    align-items: center; /* Alinha o conteúdo verticalmente no centro */
    flex-direction: column;
    margin: 0 auto;
  }

  #burger-table-heading,
  .burger-table-row {
    width: 83%; /* Ajusta a largura conforme necessário */
    align-items: center;
    display: flex;
    justify-content: center;
  }
}

@media (max-width: 299px) {
  #burger-table {
    max-width: 500px; /* Limita a largura máxima para 900px */
    display: flex;
    justify-content: center; /* Alinha o conteúdo horizontalmente no centro */
    align-items: center; /* Alinha o conteúdo verticalmente no centro */
    flex-direction: column;
    margin: 0 auto;
  }

  #burger-table-heading,
  .burger-table-row {
    width: 83%; /* Ajusta a largura conforme necessário */
    align-items: center;
    display: flex;
    justify-content: center;
  }
}
</style>
