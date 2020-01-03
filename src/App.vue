<template>
  <div id="app">
    <section class="hero is-light is-medium">
      <div class="hero-body">
        <div class="container has-text-centered">
          <h1 class="title is-2">
            To Do List!!!
          </h1>
          <h2 class="subtitle is-4">
            by Can Dagli
          </h2>
        </div>
      </div>
    </section>

    <section class="section">
      <div class="container">
        <div class="field has-addons">
          <div class="control is-expanded">
            <input
              class="input is-primary"
              type="text"
              placeholder="Type here"
              v-model="item"
              v-on:keyup.enter="add"
            />
          </div>
          <div class="control">
            <button class="button is-primary" v-on:click="add">
              Add
            </button>
          </div>
        </div>
      </div>
    </section>

    <section class="hero" v-if="is_empty">
      <div class="hero-body">
        <div class="container has-text-centered">
          <span class="is-size-4">Nothing here yet</span>
        </div>
      </div>
    </section>

    <section class="section" v-else>
      <div class="container">
        <div class="columns is-multiline">
          <div class="column is-one-fifth" v-for="(item, index) in items">
            <div class="notification is-primary">
              <button class="delete is-medium" v-on:click="remove(index)"></button>
              <span>{{ item }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

function read_cookie() {
  let cookie = document.cookie;
  if(cookie.length > 6) {
    return JSON.parse(cookie.substring(6, cookie.length));
  } else {
    return null;
  }
}

function set_cookie(items) {
  let value = JSON.stringify(items);
  document.cookie = "items=" + value;
}

export default {
  name: "app",
  components: {
    HelloWorld
  },
  data() {
    return {
      items: [],
      item: ""
    };
  },
  computed: {
    is_empty() {
      return this.items.length === 0;
    }
  },
  methods: {
    add() {
      this.items.push(this.item);
      this.item = "";
      set_cookie(this.items);
    },
    remove(index) {
      this.items.splice(index, 1);
      set_cookie(this.items);
    }
  },
  mounted() {
    let items = read_cookie();
    if (items !== null) {
      this.items = items;
    }
  }
};
</script>

<style type="text/css">
#nothing {
  padding-bottom: 2rem;
}
</style>
