<template>
  <div class="form" v-on:keyup.enter="handleClick">
    <div class="field-wrapper">
      <label for="client">Ονοματεπώνυμο</label>
      <input type="text" ref="client" id="client" v-model="client" />
    </div>
    <div class="field-wrapper">
      <label for="brand">Εταιρεία</label>
      <input type="text" ref="brand" id="brand" v-model="brand" />
    </div>
    <div class="field-wrapper">
      <label for="model">Μοντέλο</label>
      <input type="text" ref="model" id="model" v-model="model" />
    </div>
    <div class="field-wrapper">
      <label for="serial">Χρώμα</label>
      <input type="text" ref="color" id="color" v-model="color" />
    </div>
    <div class="field-wrapper">
      <label for="serial">Σειριακός</label>
      <input type="text" ref="serial" id="serial" v-model="serial" />
    </div>
    <div class="field-wrapper">
      <label for="supplier">Προμηθευτής</label>
      <select
        name="supplier"
        id="supplier"
        ref="supplier"
        @change="handleSelection"
        :class="{
          bulgaria: supplierAlias === 'bg',
          france: supplierAlias === 'fr',
        }"
      >
        <option value="atm" style="background-color: orange;"
          >atmosfaira</option
        >
        <option value="fr" style="background-color: white;">Γάλλοι</option>
        <option value="bg" style="background-color: skyblue;">Αχιλλέας</option>
      </select>
    </div>
  </div>
  <button @click="handleClick">ΣΥΝΕΧΕΙΑ</button>
  <div v-if="showModal">
    <Modal
      :modalTitle="modalHeader"
      :modalContent="modalContent"
      :brand="brand"
      :model="model"
      :color="color"
      :serial="serial"
      :clientName="client"
      :supplier="this.$refs.supplier.value"
      theme="sale"
      @close="handleClick"
    >
      <template v-slot:links>
        <a href="#">ΚΑΤΙ ΑΛΛΟ</a>
        <a href="#">ΚΑΤΙ ΑΛΛΟ</a>
      </template>
      <h3>{{ client }}</h3>
      <p>{{ serial }} - {{ supplierAlias }}</p>
    </Modal>
  </div>
</template>

<script>
  import Modal from './components/Modal.vue'
  export default {
    components: { Modal },
    name: 'App',
    data() {
      return {
        modalContent: '',
        brand: '',
        model: '',
        serial: '',
        color: '',
        client: '',
        showModal: false,
        supplierAlias: 'atm',
      }
    },
    computed: {
      modalHeader() {
        return `${this.brand} ${this.model} ${this.color}`
      },
      //   chosenSupplier() {
      //     return this.$refs.supplier.value
      //   },
    },
    methods: {
      handleClick() {
        // console.dir(this.$refs.name.value)
        // this.$refs.name.classList.add('active')
        this.showModal = !this.showModal
      },
      handleSelection() {
        this.supplierAlias = this.$refs.supplier.value
      },
    },
    mounted() {
      this.$refs.client.focus()
    },
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .form {
    margin: 0 auto;
    padding: 10px;
    width: 800px;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
    border: 4px solid rgb(7, 202, 202);
    border-radius: 14px;
  }

  button {
    margin-top: 20px;
    padding: 16px 32px;
    background: rgb(7, 202, 202);
    border-radius: 5px;
  }

  input,
  select {
    display: block;
    padding: 16px 10px;
    border: 1px solid #555;
    border-radius: 5px;
  }

  select {
    width: 170px;
    background-color: orange;
  }

  label {
    display: inline-block;
    margin-top: 20px;
  }

  .bulgaria {
    background-color: skyblue;
  }

  .france {
    background-color: white;
  }
</style>
