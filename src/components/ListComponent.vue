<template>
  <section class="list-page">
     <section class="hero">
      <div class="hero-body">
        <p class="title">
          {{ list.name }}
        </p>
      </div>
    </section>
    
    <section class="section lists">
      <input type="text" class="input mb-4" v-model="task" placeholder="Create a task..." v-on:keypress.enter.prevent="submit">
      <div :class="clsList(item)" v-on:click="markDone(item)" v-for="item in list.items" :key="item._id">
        <header class="card-header">
          <p class="card-header-title">
            {{ item.task }}
          </p>
          <div class="card-header-icon">
            <span class="icon">
              
            </span>
          </div>
        </header>
      </div>
    </section>
  </section>
</template>

<script>
export default {

  data() {
    return {
      list: null,
      task: ''
    };
  },

  methods: {
    clsList(item) {
      if (item.done) {
        return ['card', 'mb-2', 'disabled'];
      }

      return ['card', 'mb-2'];
    },
    submit() {
      const listId = this.$route.params.id;
      fetch(`/api/items/${listId}`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json'
        },
        body: JSON.stringify({ task: this.task })
      }).then(res => {
        res.json().then(() => {
          this.fetchList(listId);
          this.task = '';
        })
      })
    },
    markDone(item) {
      const listId = this.$route.params.id;
      fetch(`/api/items/${listId}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json'
        },
        body: JSON.stringify({ item: item._id })
      }).then(res => {
        res.json().then(() => {
          const index = this.list.items.findIndex(it => it._id == item._id);
          this.list.items[index].done = new Date();
        });
      });
    },
    fetchList(listId) {
      fetch(`/api/lists/${listId}`).then(res => {
        res.json().then(data => {
          this.list = data;
        });
      });
    }
  },

  mounted() {
    const listId = this.$route.params.id;
    this.fetchList(listId);
  }

}
</script>

<style scoped>
.card {
  cursor: pointer;
}

.disabled {
  background-color: #cccccc;
  text-decoration: line-through;
}
</style>