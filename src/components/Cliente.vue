<template>
  <section class="cliente">
    <div class="container">
      <h2>Novo pedido</h2>
      <div class="col-md-12 full-input">
        <span>CPF Cliente: </span>
        <input
          type="text"
          name="cpf"
          id="cpf"
          v-model="cpfInput"
          placeholder="Insira o cpf do cliente..."
        />
        <button class="btn-search" @click="getClienteByCpf">Buscar</button>
      </div>
      <div class="col-md-12 full-result" v-if="showInputCliente">
        <p>Nome Completo: {{ this.nome }} {{ this.sobrenome }}</p>
        <p>CPF: {{ this.cpf }}</p>
        <p>Data de Nascimento: {{ this.dataDeNascimento }}</p>
        <hr />
        <button class="btn-save" @click="addPedido">Salvar pedido</button>
      </div>
      <p v-if="mensagem">{{ this.aviso }}</p>
    </div>
  </section>
</template>

<script>
export default {
  name: "Cliente",
  props: {
    produtoId: String,
    valorTotal: String,
    valorUnitario: String,
    quantidade: String,
  },
  data: function() {
    return {
      cpfInput: "",
      nome: "",
      sobrenome: "",
      cpf: "",
      dataDeNascimento: "",
      showInputCliente: false,
      mensagem: false,
      aviso: "",
    };
  },
  methods: {
    getClienteByCpf: async function() {
      const result = await fetch(
        "http://localhost:3000/clientes/busca/" + this.cpfInput
      )
        .then((res) => res.json())
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      if (!result.error) {
        this.nome = result.nome;
        this.sobrenome = result.sobrenome;
        this.cpf = result.CPF;
        this.dataDeNascimento = result.dataNascimento;

        if (!this.showInputCliente) {
          this.showInputCliente = true;
        }
        if (!this.showInputCliente) {
          this.showInputCliente = true;
        }
        if (this.mensagem) {
          this.mensagem = false;
        }
      } else {
        this.showInputCliente = false;
        this.mensagem = true;
        this.aviso = "CPF inválido.";
      }
    },

    addPedido: async function() {
      const newPedido = {
        produtoId: this.produtoId,
        ValorTotal: this.valorTotal.replace(".", ","),
        valorUnitario: this.valorUnitario,
        quantidade: this.quantidade,
        clienteCPF: this.cpfInput,
      };
      const result = await fetch("http://localhost:3000/pedidos", {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify(newPedido),
      })
        .then((res) => res.json())
        .then((res) => res)
        .catch((error) => {
          return {
            error: true,
            message: error,
          };
        });
      this.showInputCliente = false;
      if (result.insertedId) {
        this.mensagem = true;
        this.aviso = "Pedido cadastrado com sucesso.";
      }
    },
  },
};
</script>

<style>
.btn-search {
  width: 90px;
  height: 40px;
  border-radius: 6px;
  background-color: #ae382b;
  color: #f5a022;
  border: none;
  font-weight: bold;
}

.btn-search:hover {
  cursor: pointer;
}

#cpf {
  width: 280px;
  margin-left: 20px;
}

.btn-save {
  width: 170px;
  height: 40px;
  border-radius: 6px;
  background-color: #ae382b;
  color: #f5a022;
  border: none;
  font-weight: bold;
  display: block;
  margin: 30px auto;
}

.btn-save:hover {
  cursor: pointer;
}
</style>
