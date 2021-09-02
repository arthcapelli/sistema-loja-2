<template>
  <section class="detalhe">
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <h2>Detalhe do Produto</h2>
          <img :src="img" alt="" />
        </div>
        <div class="col-md-9">
          <h2>{{ title }}</h2>
          <p>R$ {{ price }}</p>
        </div>
        <div class="col-md-3">
          <div class="detalhe__box-price">
            <p>Quantidade</p>
            <input
              @input="toCalculate"
              type="number"
              v-model="quantity"
              min="0"
            />
          </div>
        </div>
        <div class="col-md-12">
          <hr />
          <h3>Descrição:</h3>
          <p>
            Lorem Ipsum is simply dummy text of the printing and typesetting
            industry. Lorem Ipsum has been the industry's standard dummy text
            ever since the 1500s, when an unknown printer took a galley of type
            and scrambled it to make a type specimen book. It has survived not
            only five centuries, but also the leap into electronic typesetting,
            remaining essentially unchanged. Lorem Ipsum is simply dummy text of
            the printing and typesetting industry. Lorem Ipsum has been the
            industry's standard dummy text ever since the 1500s, when an unknown
            printer took a galley of type and scrambled it to make a type
            specimen book. It has survived not only five centuries, but also the
            leap into electronic typesetting, remaining essentially unchanged
          </p>
          <h4>
            Total : {{ finalQuantity }} * {{ price }} =
            {{ total.toString().replace('.', ',') }}
          </h4>
        </div>
        <div class="col-md-12">
          <button class="pedido-btn" @click="switchVisibility">
            Fazer Pedido
          </button>
        </div>
        <div class="row">
          <div class="col-md-12 cpf-input" v-if="showInput">
            <hr />
            <Cliente
              :produtoId="this.id"
              :valorTotal="this.total"
              :valorUnitario="this.price"
              :quantidade="this.quantity"
            />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Cliente from './Cliente.vue';
export default {
  name: 'Detalhe',
  components: {
    Cliente,
  },
  props: {
    title: String,
    price: String,
    img: String,
    id: String,
  },
  data: function() {
    return {
      quantity: 0,
      finalQuantity: 0,
      total: 0,
      showInput: false,
    };
  },
  methods: {
    switchVisibility() {
      if (!this.showInput) {
        this.showInput = true;
      } else {
        this.showInput = false;
      }
    },
    toCalculate: function() {
      this.finalQuantity = this.quantity;
      if (this.quantity === '') {
        this.finalQuantity = 1;
      }

      if (this.finalQuantity < 0) {
        this.total = 0;
      } else {
        const total = parseFloat(this.price) * this.finalQuantity;

        this.total = total.toFixed(2);
      }
    },
  },
};
</script>
<style>
.detalhe {
  padding: 50px 0px;
}

.detalhe img {
  margin: 20px auto;
  display: block;
}

.detalhe__box-price {
  text-align: right;
  font-weight: bold;
}

.detalhe input {
  width: 50px;
  height: 30px;
  border-radius: 4px;
  padding: 0px 8px;
}

.pedido-btn {
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

.pedido-btn:hover {
  cursor: pointer;
}

.full-input {
  display: flex;
  flex-direction: row;
  align-items: center;
}
</style>
