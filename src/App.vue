<template>
<table>
      <thead>
        <tr>
          <th>Személy</th>
          <th>Magasság</th>
          <th>Ár</th>
          <th>Műveletek</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="statue in statueList" v-bind:key="statue.id">
          <td>{{ statue.person }}</td>
          <td>{{ statue.height }}</td>
          <td>{{ statue.price }}</td>
          <td>
            <button>Törlés</button>
            <button>Szerkesztés</button>
          </td>
        </tr>

        <tr>
          <td>
            <input type="hidden" v-model="statue.id">
          </td>
          <td>
            <input type="text" v-model="statue.person">
          </td>
          <td>
            <input type="number" v-model="statue.height">
          </td>
          <td>
            <input type="number" v-model="statue.price">
          </td>
          <td>
            <button @click="newStatue">Létrehoz</button>
            <button @click="saveStatue">Mentés</button>
          </td>
        </tr>
      </tbody>
    </table>
</template>

<script>
export default {
  name: "Statues",
  data() {
    return {
      statue: {
        id: null,
        person: '',
        height: null,
        price: null
      },
      statueList:[]
    }
  },
  methods: {
    async loadData() {
     let Response = await fetch('http://127.0.0.1:8000/api/statues')
     let data = await Response.json()
     this.statueList = data
    },

    async newStatue() {
      this.saving='disabled'
     await fetch('http://127.0.0.1:8000/api/statues', {
       method: 'POST',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.statue) 
     })
     await this.loadData()
     this.saving=false
    },
    async saveStatue() {
      this.saving='disabled'
     await fetch(`http://127.0.0.1:8000/api/statues/${this.statue.id}`, {
       method: 'PATCH',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.statue) 
     })
     await this.loadData()
     this.saving=false
    },
  },
  mounted() {
    this.loadData()
  }
}
</script>