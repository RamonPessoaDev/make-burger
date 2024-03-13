<template>
  <div>
    <div class="msg-container">
      <Message :msg="msg" v-show="msg" />
    </div>
    <div>
      <form id="burger-form" @submit="createBurger">
        <div class="input-container">
          <label for="nome">Nome do Cliente:</label>
          <input
            type="text"
            id="nome"
            name="nome"
            v-model="nome"
            placeholder="Digite o seu nome"
          />
        </div>
        <div class="input-container">
          <label for="pao">Escolha o pão:</label>
          <select name="pao" id="pao" v-model="pao">
            <option value="" selected disabled>Selecione o seu pão</option>
            <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
              {{ pao.tipo }}
            </option>
          </select>
        </div>
        <div class="input-container">
          <label for="carne" selected disabled
            >Escolha a carne do seu Burger:</label
          >
          <select name="carne" id="carne" v-model="carne">
            <option value="" selected disabled>
              Selecione o tipo de carne
            </option>
            <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
              {{ carne.tipo }}
            </option>
          </select>
        </div>
        <div id="opcionais-container" class="input-container">
          <label id="opcionais-title" for="opcionais" selected disabled
            >Selecione os opcionais:</label
          >
          <div
            class="checkbox-container"
            v-for="opcional in opcionaisdata"
            :key="opcional.id"
          >
            <input
              type="checkbox"
              name="opcionais"
              v-model="opcionais"
              :value="opcional.tipo"
            />
            <span>{{ opcional.tipo }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Criar meu Burger!" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "BurgerForm",
  data() {
    return {
      //dados que vem do servidor
      paes: null,
      carnes: null,
      opcionaisdata: null,
      //dados enviados pro servidor
      nome: null,
      pao: "",
      carne: "",
      opcionais: [],
      msg: null
    };
  },
  methods: {
    async getIngredients() {
      const req = await fetch(
        "https://makeburger-api.onrender.com/ingredientes"
      );
      const data = await req.json();

      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault();

      const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado"
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch("https://makeburger-api.onrender.com/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson
      });

      const res = await req.json();

      //Colocar msg de sistema
      this.msg = `Pedido Nº <strong>${res.id}</strong> realizado com sucesso!`;

      //Limpar msg depois de 3seg
      setTimeout(() => (this.msg = ""), 3000);

      // Limpa os campos
      this.nome = "";
      this.carne = "";
      this.pao = "";
      this.opcionais = [];
    }
  },
  mounted() {
    this.getIngredients();
  },
  components: {
    Message
  }
};
</script>

<style scoped>
#burger-form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 10px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
  margin-left: 50px;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container input {
  cursor: pointer;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: #5c5b5b;
  color: #222;
}

.msg-container {
  display: flex;
  align-items: center;
  justify-content: center;
}

.message {
  text-align: center;
}

@media (max-width: 1024px) {
  .checkbox-container {
    justify-content: start;
    align-items: center;
  }
}

@media (max-width: 600px) {
  #burger-form {
    max-width: 100%;
  }

  #opcionais-container {
    margin-left: 0px;
  }

  #opcionais-container div {
    padding: 5px 0px;
  }

  .checkbox-container {
    justify-content: start;
    align-items: center;
  }

  .input-container {
    margin-left: auto;
    margin-right: auto;
  }
}

@media (max-width: 425px) {
  .checkbox-container {
    justify-content: start;
    align-items: center;
  }

  #opcionais-container {
    margin-left: 5px;
  }
}

@media (max-width: 375px) {
  #burger-form {
    display: flex;
    justify-content: center; /* Alinha o conteúdo horizontalmente no centro */
    align-items: center; /* Alinha o conteúdo verticalmente no centro */
    flex-direction: column;
    margin: 0 auto;
  }

  #burger-form .input-container {
    justify-content: flex-start; /* Alinha o conteúdo horizontalmente no centro */
    align-items: start; /* Alinha o conteúdo verticalmente no centro */
    flex-direction: row;
    flex-wrap: wrap;
  }

  #opcionais-container {
    margin-left: 0px;
  }

  #opcionais-container div {
    padding: 4px 0px;
  }

  .checkbox-container {
    justify-content: flex-start;
    align-items: center;
  }

  .input-container {
    margin-right: 0;
  }
}

@media (max-width: 299px) {
  #burger-form {
    display: flex;
    justify-content: center; /* Alinha o conteúdo horizontalmente no centro */
    align-items: center; /* Alinha o conteúdo verticalmente no centro */
    flex-direction: column;
    margin: 0 auto;
  }

  #burger-form .input-container {
    justify-content: flex-start; /* Alinha o conteúdo horizontalmente no centro */
    align-items: start; /* Alinha o conteúdo verticalmente no centro */
    flex-direction: row;
    flex-wrap: wrap;
    width: 110%;
    margin-left: 10px;
  }

  #opcionais-container {
    margin-left: 0px;
  }

  #opcionais-container div {
    padding: 2px 0px;
  }

  .checkbox-container {
    justify-content: flex-start;
    align-items: center;
  }

  .input-container {
    margin-right: 0;
  }
}
</style>
