<template>
  <div>

    <h1>Plüssök</h1>
    <table>
      <thead>
        <tr>
          <th>Név</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="pluss in plussok" v-bind:key="pluss.id">
          <td>{{ pluss.id }}</td>
          <td>{{ pluss.name }}</td>
          <td>
            <button @click="deletePluss(pluss.id)">Törlés</button>
            <button @click="editPluss(pluss.id)">Szerkesztés</button>
          </td>
        </tr>
        <tr>
          <td>
            <input type="hidden" v-model="pluss.id">
          </td>
          <td>
            <input type="text" v-model="pluss.name">
          </td>
          <td>
            <button v-if="mod_new" @click="newPluss" :disabled="saving">Létrehoz</button>
            <button v-if="!mod_new" @click="savePluss" :disabled="saving">Mentés</button>
            <button v-if="!mod_new" @click="cancelEdit" :disabled="saving">Mégse</button>
          </td>
        </tr>
      </tbody>
    </table>

    <!--<button @click="loadData">Újratöltés</button> -->
  </div>
</template>



<script>
export default {
  name: 'Pluss',
  components: {
  },
  data() {
    return {
      mod_new: true, 
      saving: false,
      pluss: {
        id: null,
        name: '',
      },
      plussok: []
    }
  },
  methods: {
    async loadData () {
     let Response = await fetch('http://127.0.0.1:8000/api/pluss')
     let data = await Response.json()
     this.plussok = data
    },
    async deletePluss(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/pluss/${id}`, {
        method: 'DELETE'
      })
      console.log(Response)
      await this.loadData()
    },
    async newPluss() {
      this.saving='disabled'
     await fetch('http://127.0.0.1:8000/api/pluss', {
       method: 'POST',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.pluss) 
     })
     await this.loadData()
     this.saving=false
     this.resetForm()
    },
    async savePluss() {
      this.saving='disabled'
     await fetch(`http://127.0.0.1:8000/api/pluss/${this.pluss.id}`, {
       method: 'PUT',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.pluss) 
     })
     await this.loadData()
     this.saving=false
     this.resetForm()
    },
    async editPluss(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/pluss/${id}`)
      let data = await Response.json()
      this.pluss = {...data};
      this.mod_new = false
    },
    cancelEdit () {
      this.resetForm()
    },
    resetForm() {
      this.pluss = {
        id: null,
        title: '',
      }
      this.mod_new = true
    }
  },
  mounted() {
    this.loadData()
  }
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
</style>