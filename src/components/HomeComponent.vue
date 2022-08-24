<template>
  <section class="lists-page">
    <section class="hero">
      <div class="hero-body">
        <p class="title">
          TO-DO Lists ✨
        </p>
        <p class="subtitle">
          This is a list of to-do lists !
        </p>
      </div>
    </section>
    
    <section class="section lists">
      <input type="text" class="input mb-4" v-model="todo" placeholder="Create a to-do list..." v-on:keypress.enter.prevent="submit" />
      <router-link :to="{ name: 'list', params: { id: list._id } }" tag="div" class="card mb-2" v-for="list in lists" :key="list._id">
        <header class="card-header">
          <p class="card-header-title">
            {{ list.name }}
          </p>
          <div class="card-header-icon">
            <span class="icon">
              ➡️
            </span>
          </div>
        </header>
      </router-link>
    </section>
  </section>
</template>

<script>
export default {

  data() {
    return {
      lists: [],
      todo: ''
    }
  },

  mounted() {
    this.fetchLists();
  },

  methods: {
    fetchLists() {
      fetch('/api/lists/').then(res => {
        res.json().then(lists => {
          this.lists = lists;
        });
      });
    },
    submit() {
      fetch('/api/lists/', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json'
        },
        body: JSON.stringify({
          name: this.todo
        })
      }).then(res => {
          res.json().then(() => {
            this.fetchLists();
            this.todo = '';
          });
      });
    }
  }

}
</script>

<style scoped>
  .card {
    cursor: pointer;
  }
</style>