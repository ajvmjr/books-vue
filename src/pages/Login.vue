<template>
  <div class="container-principal">
    <section class="container-principal__section">
      <form @submit="Submit">
        <input
          :class="{ fieldError: erros.email }"
          type="email"
          placeholder="Email"
          v-model="email"
          @blur="checkFields"
        />
        <input
          :class="{ fieldError: erros.senha }"
          type="password"
          placeholder="Senha"
          v-model="senha"
          @blur="checkFields"
        />
        <label for="" @click="showMessage">Esqueci minha senha</label>

        <button class="btn-submit">ENTRAR</button>
      </form>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      name: "",
      email: "",
      senha: "",
      erros: {
        email: false,
        senha: false,
      },
    };
  },

  methods: {
    async Submit(e) {
      e.preventDefault();

      const { data } = await axios.get("http://localhost:3000/users", {
        params: {
          email: this.email,
          senha: this.senha,
        },
      });

      if (data.length > 0) {
        this.$router.push({ name: "dashboard" });
      } else {
        alert("Email ou senhas incorretos.");
      }
    },

    checkFields() {
      if (this.email === "") {
        this.erros.email = true;
      } else {
        this.erros.email = false;
      }

      if (this.senha === "") {
        this.erros.senha = true;
      } else {
        this.erros.senha = false;
      }
    },

    showMessage(e) {
      e.preventDefault();

      alert("Desculpe, funcionalidade ainda não desenvolvida.");
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap");

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.fieldError {
  border: 2px solid red;
}

.container-principal {
  align-items: center;
  background: #212121;
  display: flex;
  height: 100vh;
  justify-content: center;
  width: 100vw;
}

.container-principal__section {
  align-items: center;
  /* background-color: #fff; */
  display: flex;
  height: 80vh;
  justify-content: center;
  width: 30vw;
}

form {
  /* background-color: whitesmoke; */
  display: flex;
  flex-direction: column;
  height: 50%;
  justify-content: space-around;
  width: 80%;
}

input {
  background: #ffffff;
  border-radius: 5px;
  border: none;
  padding: 20px;
}

::placeholder {
  font-family: "Roboto", sans-serif;
  font-style: normal;
  font-size: 16px;
  font-weight: normal;
  letter-spacing: 0.15px;
  line-height: 24px;
}

label {
  color: #c4c4c4;
  font-family: "Roboto", sans-serif;
  font-size: 14px;
  font-style: normal;
  font-weight: normal;
  letter-spacing: 0.15px;
  line-height: 24px;
  padding-left: 5px;
}

label:hover {
  cursor: pointer;
}

.btn-submit {
  background: #6202ee;
  border-radius: 5px;
  color: #ffffff;
  border: 0;
  font-family: "Roboto", sans-serif;
  font-size: 16px;
  font-style: normal;
  font-weight: 500;
  letter-spacing: 0.15px;
  line-height: 24px;
  margin-top: 25px;
  padding: 15px;
}

.btn-submit:hover {
  cursor: pointer;
  opacity: 0.9;
}

.btn-submit:focus {
  outline: 0;
}
</style>

==
