<template>
  <div class="form" v-on:keyup.enter="handleClick">
    <label for="client">Ονοματεπώνυμο</label>
    <input type="text" ref="client" id="client" v-model="client" />
    <label for="brand">Εταιρεία</label>
    <input type="text" ref="brand" id="brand" v-model="brand" />
    <label for="model">Μοντέλο</label>
    <input type="text" ref="model" id="model" v-model="model" />
    <label for="serial">Χρώμα</label>
    <input type="text" ref="color" id="color" v-model="color" />
    <label for="serial">Σειριακός</label>
    <input type="text" ref="serial" id="serial" v-model="serial" />
    <button @click="handleClick">Show Modal</button>
  </div>
  <div v-if="showModal">
    <Modal
      :modalTitle="modalHeader"
      :modalContent="modalContent"
      :brand="brand"
      :model="model"
      :color="color"
      :serial="serial"
      :clientName="client"
      theme=""
      @close="handleClick"
    >
      <template v-slot:links>
        <a href="">Sign Up Now</a>
        <a href="">More Info</a>
      </template>
      <!-- <h1>Slot Title</h1>
      <p>Slot Content</p> -->
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
        modalContent: 'Πατα το κουμπί για να προχωρήσεις',
        brand: '',
        model: '',
        serial: '',
        color: '',
        client: '',
        showModal: false,
      }
    },
    computed: {
      modalHeader() {
        return `${this.brand} ${this.model} ${this.color}`
      },
    },
    methods: {
      handleClick() {
        // console.dir(this.$refs.name.value)
        // this.$refs.name.classList.add('active')
        this.showModal = !this.showModal
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
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .form button {
    margin-top: 20px;
    padding: 16px 32px;
    background: rgb(7, 202, 202);
    border-radius: 5px;
  }

  input {
    display: block;
    padding: 16px 10px;
    border: 1px solid #555;
    border-radius: 5px;
  }

  label {
    display: inline-block;
    margin-top: 20px;
  }
</style>
