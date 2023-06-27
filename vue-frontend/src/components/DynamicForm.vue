<template>
  <h3>123</h3>
  <div>
      <input v-model="habitField" placeholder="Habit" type="text" ref="habitInput">
      <input v-model="targetField" placeholder="Target" @keyup.enter="save()">
      <button type="button" @click="save()">Save</button>
  </div>
  <div>
      <table>
          <thread>
              <tr>
                  <th>Habit</th>
                  <th>Target</th>
              </tr>
          </thread>
          <body>
          <tr v-if="items.length === 0">
              <td colspan="2">No habits yet</td>
          </tr>
          <tr v-for="item in items" :key="item.id">
              <td>{{item.habit}}</td>
              <td>{{item.target}}</td>
          </tr>
          <tr>
              <td>{{item.habitField}}</td>
              <td>{{item.targetField}}</td>
          </tr>
          </body>
      </table>
  </div>
</template>

<script>
export default {
    name: 'DynamicForm',
    props: ['title'],
    data() {
        return {
            items: [],
            habitField: '',
            targetField: ''
        }
    },
    methods: {
        loadHabits() {
            const endpoint = 'http://localhost:8080/habits'
            const requestOptions = {
                method: 'GET',
                redirect: 'follow'
            }
            fetch(endpoint, requestOptions)
                .then(response => response.json())
                .then(result => result.forEach(habit => {
                    this.items.push(habit)
                }))
                .catch(error => console.log('error', error))
        },
        save() {
            const endpoint = 'http://localhost:8080/add'
            const data = {
                habit: this.habitField,
                target: this.targetField
            }
            const requestOptions = {
                method: 'POST',
                headers:{
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            }
            fetch(endpoint, requestOptions)
                .then(response => response.json())
                .then(data => {
                    console.log('Success:', data)
                })
                .catch(error => console.log('error', error))
        }
    },
    async created () {
        await this.setup()
        this.loadHabits()
    },
    mounted () {
    },
    updated() {
        console.log("UPDATED!")
    }
}
</script>

<style scoped>

</style>