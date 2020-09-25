<template>
  <div class="container">
    <header class="container__header">
      <div class="content flex flex--space-between">
        <img src="@/assets/logo-signa.png" alt="logo da signa" />

        <section class="container__header__section">
          <p>
            <!-- {{ $route.params.name }} -->
            Nome da pessoa
          </p>

          <img
            src="@/assets/logout.png"
            alt="logout"
            class="container__header__section__img-logout"
            @click="Logout"
          />
        </section>
      </div>
    </header>

    <h3 class="container__title content margin-bottom">
      Lista de Livros
    </h3>

    <div class="content">
      <table class="table">
        <thead>
          <tr>
            <th>Id</th>
            <th>Autor</th>
            <th>Título</th>
            <th>Editora</th>
            <th>Ano de edição</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="book in listOfBooks" :key="book.id">
            <td>{{ book.id }}</td>
            <td>{{ book.author }}</td>
            <td>{{ book.title }}</td>
            <td>{{ book.publishingcompany }}</td>
            <td>{{ book.editionyear }}</td>
            <td>
              <a href="#">
                <img
                  src="@/assets/edit.png"
                  alt="Editar"
                  class="table__edit"
                  @click="
                    ModalEditar = true;
                    populateInput($event, book);
                  "
                />
              </a>
              <a href="#">
                <img
                  src="@/assets/delete.png"
                  alt="Deletar"
                  class="table__delete"
                  @click="
                    openDelete($event, true);
                    deleteId = book.id;
                  "
                />
              </a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <section class="container__btns content">
      <button class="container__btns__btn-new" @click="Modal = true">
        Novo
      </button>
    </section>

    <Modal v-if="Modal">
      <div class="modal-adicionar">
        <section class="modal-adicionar__header">
          <h4 class="modal-adicionar__header__title">
            Cadastrar novo Livro
          </h4>
          <a href="#" @click="CloseModal">
            <img src="@/assets/close.png" alt="Fechar modal" />
          </a>
        </section>
        <form class="modal-adicionar__form" @submit="RegisterBook">
          <input
            type="text"
            placeholder="Autor"
            v-model="criar.autor"
            class="modal-adicionar__form__input"
          />
          <input
            type="text"
            placeholder="Título"
            v-model="criar.titulo"
            class="modal-adicionar__form__input"
          />
          <input
            type="text"
            placeholder="Editora"
            v-model="criar.editora"
            class="modal-adicionar__form__input"
          />
          <input
            type="text"
            placeholder="Ano de edição"
            v-model="criar.anoedicao"
            class="modal-adicionar__form__input"
          />
          <button class="modal-adicionar__form__btn-voltar" @click="CloseModal">
            Voltar
          </button>
          <button class="modal-adicionar__form__btn-add">
            CADASTRAR
          </button>
        </form>
      </div>
    </Modal>

    <Modal v-if="ModalEditar">
      <div class="modal-adicionar">
        <section class="modal-adicionar__header">
          <h4 class="modal-adicionar__header__title">
            Editar funcionário
          </h4>
          <a href="#" @click="CloseModal">
            <img src="@/assets/close.png" alt="Fechar modal" />
          </a>
        </section>

        <form class="modal-adicionar__form" @submit="UpdateBook">
          <input
            type="text"
            placeholder="Autor"
            v-model="editar.autor"
            class="modal-adicionar__form__input"
          />
          <input
            type="text"
            placeholder="Título"
            v-model="editar.titulo"
            class="modal-adicionar__form__input"
          />
          <input
            type="text"
            placeholder="Editora"
            v-model="editar.editora"
            class="modal-adicionar__form__input"
          />
          <input
            type="text"
            placeholder="Ano de edição"
            v-model="editar.anoedicao"
            class="modal-adicionar__form__input"
          />
          <button class="modal-adicionar__form__btn-voltar" @click="CloseModal">
            Voltar
          </button>
          <button class="modal-adicionar__form__btn-add">
            SALVAR
          </button>
        </form>
      </div>
    </Modal>

    <Modal v-if="ModalDeletar">
      <div class="modal-deletar">
        <section class="modal-deletar__header">
          <h4 class="modal-deletar__header__title">
            Tem certeza que deseja deletar?
          </h4>
          <a href="#" @click="CloseModal">
            <img src="@/assets/close.png" alt="Fechar modal" />
          </a>
        </section>

        <div class="modal-deletar__btns flex">
          <button
            class="modal-deletar__btns__btn-add"
            @click="DeleteBook($event, deleteId)"
          >
            Deletar
          </button>
          <button class="modal-deletar__btns__btn-voltar" @click="CloseModal">
            Cancelar
          </button>
        </div>
      </div>
    </Modal>
  </div>
</template>

<script>
import axios from "axios";
import Modal from "@/components/common/Modal";

export default {
  components: {
    Modal,
  },

  data() {
    return {
      listOfBooks: [],
      deleteId: "",
      Modal: false,
      ModalEditar: false,
      ModalDeletar: false,
      criar: {
        autor: "",
        titulo: "",
        editora: "",
        anoedicao: "",
      },
      editar: {
        id: "",
        autor: "",
        titulo: "",
        editora: "",
        anoedicao: "",
      },
    };
  },

  mounted() {
    this.GetBooks();
  },

  methods: {
    async GetBooks() {
      const { data } = await axios.get("http://localhost:3000/books");
      console.log(data);
      this.listOfBooks = data;
    },

    async DeleteBook(e, id) {
      e.preventDefault();

      await axios.delete(`http://localhost:3000/books/${id}`);

      this.GetBooks();
      this.CloseModal(e);
    },

    async RegisterBook(e) {
      e.preventDefault();

      await axios.post("http://localhost:3000/books/", {
        author: this.criar.autor,
        title: this.criar.titulo,
        publishingcompany: this.criar.editora,
        editionyear: this.criar.anoedicao,
      });

      this.criar.autor = "";
      this.criar.titulo = "";
      this.criar.editora = "";
      this.criar.anoedicao = "";

      this.GetBooks();
      this.CloseModal(e);
    },

    async UpdateBook(e) {
      e.preventDefault();

      await axios.put(`http://localhost:3000/books/${this.editar.id}`, {
        author: this.editar.autor,
        title: this.editar.titulo,
        publishingcompany: this.editar.editora,
        editionyear: this.editar.anoedicao,
      });

      this.editar.autor = "";
      this.editar.titulo = "";
      this.editar.editora = "";
      this.editar.anoedicao = "";

      this.GetBooks();
      this.CloseModal(e);
    },

    populateInput(e, book) {
      e.preventDefault();

      this.editar.id = book.id;
      this.editar.autor = book.author;
      this.editar.titulo = book.title;
      this.editar.editora = book.publishingcompany;
      this.editar.anoedicao = book.editionyear;
    },

    openDelete(e, value) {
      e.preventDefault();

      this.ModalDeletar = true;
    },

    CloseModal(e) {
      e.preventDefault();

      this.criar.autor = "";
      this.criar.titulo = "";
      this.criar.editora = "";
      this.criar.anoedicao = "";

      this.Modal = false;
      this.ModalEditar = false;
      this.ModalDeletar = false;
    },

    Logout(e) {
      e.preventDefault();

      this.$router.push({ name: "login" });
    },
  },
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@700&family=Roboto:wght@300&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@700&family=Roboto:wght@300&family=Source+Sans+Pro&display=swap");

.content {
  width: 80%;
  margin: 0 auto;
}

.flex {
  display: flex;

  &--space-between {
    justify-content: space-between;
  }

  &--align-items {
    align-items: center;
  }
}

.margin-bottom {
  margin-bottom: 30px;
}

.container {
  background: #212121;
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 100%;

  &__header {
    align-items: center;
    display: flex;
    justify-content: space-between;
    padding: 30px;
    width: 100%;

    &__section {
      align-items: center;
      display: flex;

      p {
        color: #ffffff;
        font-family: "Roboto";
        font-style: normal;
        font-size: 16px;
        font-weight: 500;
        line-height: 24px;
        letter-spacing: 0.15px;
      }

      &__img-logout {
        margin-left: 15px;

        &:hover {
          cursor: pointer;
        }
      }
    }
  }

  &__title {
    color: #ffffff;
    font-family: "Poppins", sans-serif;
    font-style: normal;
    font-weight: bold;
    font-size: 24px;
    line-height: 125%;
  }

  &__btns {
    display: flex;
    justify-content: flex-end;
    padding-top: 30px;

    &__btn-new {
      background-color: #6202ee;
      border: 0;
      border-radius: 5px;
      color: #ffffff;
      font-family: "Roboto";
      font-weight: 500;
      font-size: 16px;
      line-height: 24px;
      letter-spacing: 0.15px;
      padding: 2vh 5vw;

      &:hover {
        cursor: pointer;
        opacity: 0.9;
      }
    }
  }

  .modal-adicionar {
    display: flex;
    flex-direction: column;
    padding: 15px;
    height: 100%;
    width: 100%;

    &__header {
      align-items: center;
      display: flex;
      border-bottom: 1px solid rgba(255, 255, 255, 0.5);
      justify-content: space-between;
      padding: 10px;
      &__title {
        color: #ffffff;
        font-family: Poppins;
        font-style: normal;
        font-weight: bold;
        font-size: 24px;
        line-height: 125%;
      }
    }

    &__form {
      display: flex;
      flex-wrap: wrap;
      height: 75%;
      justify-content: flex-end;
      padding: 10px;

      &__input {
        background: #ffffff;
        border-radius: 5px;
        border: 0;
        margin-bottom: 10px;
        outline: 0;
        padding: 20px;
        width: calc(50% - 5px);

        &:nth-child(1) {
          margin-right: 5px;
        }
        &:nth-child(3) {
          margin-right: 5px;
        }
      }

      &__btn-voltar {
        background: transparent;
        border: 1px solid #fff;
        padding: 10px;
        border-radius: 5px;
        color: #ffffff;
        cursor: pointer;
        font-family: "Roboto";
        font-style: normal;
        font-weight: 500;
        line-height: 24px;
        letter-spacing: 0.15px;
        outline: 0;
        padding: 10px;
        text-decoration: none;
        transition: 800ms;
        width: 10vw;
        &:hover {
          background: #fff;
          color: #000;
        }
      }

      &__btn-add {
        background: rgb(98, 2, 238);
        border: 0;
        border-radius: 5px;
        color: #fff;
        cursor: pointer;
        font-family: "Roboto";
        font-style: normal;
        font-weight: 500;
        line-height: 24px;
        letter-spacing: 0.15px;
        margin-left: 10px;
        outline: 0;
        padding: 10px;
        transition: 800ms;
        width: 10vw;
        &:hover {
          background: rgb(66, 0, 165);
        }
      }
    }
  }

  .modal-deletar {
    display: flex;
    flex-direction: column;
    padding: 15px;
    height: 100%;
    width: 100%;

    &__header {
      align-items: center;
      display: flex;
      border-bottom: 1px solid rgba(255, 255, 255, 0.5);
      justify-content: space-between;
      padding: 10px;
      &__title {
        color: #ffffff;
        font-family: Poppins;
        font-style: normal;
        font-weight: bold;
        font-size: 24px;
        line-height: 125%;
      }
    }

    &__btns {
      align-items: center;
      flex-direction: column;
      justify-content: space-evenly;
      height: 75%;
      width: 100%;

      &__btn-voltar {
        background: transparent;
        border: 1px solid #fff;
        padding: 10px;
        border-radius: 5px;
        color: #ffffff;
        cursor: pointer;
        font-family: "Roboto";
        font-style: normal;
        font-weight: 500;
        line-height: 24px;
        letter-spacing: 0.15px;
        outline: 0;
        padding: 10px;
        text-decoration: none;
        transition: 800ms;
        width: 50%;
        &:hover {
          background: #fff;
          color: #000;
        }
      }

      &__btn-add {
        background: rgb(98, 2, 238);
        border: 0;
        border-radius: 5px;
        color: #fff;
        cursor: pointer;
        font-family: "Roboto";
        font-style: normal;
        font-weight: 500;
        line-height: 24px;
        letter-spacing: 0.15px;
        outline: 0;
        padding: 10px;
        transition: 800ms;
        width: 50%;
        &:hover {
          background: rgb(66, 0, 165);
        }
      }
    }
  }
}

.table {
  background-color: #303030;
  border-collapse: collapse;
  justify-content: space-evenly;
  width: 100%;

  &__edit {
    margin-right: 15px;
    &:hover {
      cursor: pointer;
    }
  }

  &__delete {
    &:hover {
      cursor: pointer;
    }
  }

  thead {
    tr {
      th {
        color: #f7f8f9;
        font-family: "Source Sans Pro", sans-serif;
        font-style: normal;
        font-weight: 600;
        font-size: 14px;
        line-height: 150%;
        padding: 10px 0;
      }
    }
  }

  tbody {
    tr {
      td {
        color: #f7f8f9;
        border-top: 2px solid #e5e5e5;
        font-family: "Source Sans Pro", sans-serif;
        font-style: normal;
        font-weight: normal;
        font-size: 14px;
        line-height: 150%;
        padding: 15px 0;
        text-align: center;
      }
    }
  }
}
</style>
