<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>
    <h3>Habit Tracker</h3>
    <div>
        <input v-model="habitField" placeholder="Habit" type="text" ref="habitInput">
        <input v-model="targetField" placeholder="Target" @keyup.enter="save()">
        <button type="button" @click="save()">Save</button>
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
        },
        async setup () {
            if (this.$root.authenticated) {
                this.claims = await this.$auth.getUser()
                // this.accessToken = await this.$auth.getAccessToken()
            }
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


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
<script setup>
</script>
<script setup>
</script>
<script setup>
</script>