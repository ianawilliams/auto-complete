<template>
  <div id="app">
    <div class="header">
      <a class="home-link" href="https://ianawilliams.github.io/">
        <span class="material-icons search-bar__icon">home</span>
      </a>
      <h1>Recipe Auto Complete</h1>
    </div>
    <main>
      <div class="main-item">
        <div class="main-item__label">
          1. Absolute:
        </div>
        <AutoCompleteVue absolute :options="list" place-holder="Search..." />
      </div>
      <div class="main-item">
        <div class="main-item__label">
          2. Relative:
        </div>
        <AutoCompleteVue :options="list" place-holder="Search..." />
      </div>
      <div class="main-item">
        <div class="main-item__label">
          3. Absolute - Suggest All:
        </div>
        <AutoCompleteVue suggestAll absolute :options="list" place-holder="Search..." />
      </div>
      <div class="main-item">
        <div class="main-item__label">
          4. Absolute - Suggest All - Max Height 150px:
        </div>
        <AutoCompleteVue absolute max-height="150px" suggestAll :options="list" place-holder="Search..." />
      </div>
      <div class="main-item">
        <div class="main-item__label">
          5. Absolute - custom filter
        </div>
        <AutoCompleteVue 
          absolute
          place-holder="Search..."
          :loading="loading"
          disable-filter
          :over-ride-results="apiResults"
          @search-term="loadResults"
        />

        <p style="font-size: 13px;">This filter function is called from the parent of the auto complete component. It
            Allows the developer to make a custom API call. This example just has a 1.5s delay on 
            removing the loading animation. Debounce is called in Parent component on fake API call.
        </p>
      </div>
    </main>
    <footer>
      <a href="https://github.com/ianawilliams/auto-complete" target="_blank">Link to Git Hub Repo</a>
    </footer>
  </div>
</template>

<script>
import AutoCompleteVue from '@/components/AutoComplete.vue'
import debounce from 'lodash/debounce';

export default {
  name: 'App',
  components: {
    AutoCompleteVue
  },
  data() {
    return {
      loading: false,
      apiResults: [],
      searchTerm: "",
      list: [
        "Ratatouille",
        "Pizza",
        "Pasta",
        "Pie",
        "Burger",
        "Ramen",
        "Fries",
        "Spaghetti",
        "Chicken Nuggets",
        "Shrimp",
        "Tamale",
        "Chicken cordon bleu",
        "Chicken Parm",
        "Fried Rice",
        "Fried Chicken",
      ],
    }
  },
  methods: {
    apiCall: debounce(async function(val) {
      this.apiResults = await new Promise((resolve) => {
        setTimeout(() => {
          resolve(
            this.list.filter((item) => {
              return item.toLocaleLowerCase().includes(val.toLocaleLowerCase())
            })
          )
        }, 1500)
      });
      this.loading = false;
    }, 300),

    async loadResults(val) {
      if (val) {
        this.loading = true;
        this.apiCall(val);
      } else {
        this.apiResults = [];
      }
    }
  }
}
</script>

<style lang="scss">
@import 'https://fonts.googleapis.com/icon?family=Material+Icons';
@import '@/styles/scss/index.scss';
body,
html,
#app {
  height: 100vh
}

body {
  margin: 0;
  background: #eaeaea;
}

#app {
  display: flex;
  flex-direction: column;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

  a {
    text-decoration: none;
    color: #FFF;

    span {
      display: flex;
    }
  }
}

main {
  flex-grow: 1;
  width: 100%;
  box-sizing: border-box;
  padding: 20px;
}

.main-item {
  margin-bottom: 20px;

  .main-item__label {
    font-size: 13px;
    color: $primary;
    font-weight: bold;
    margin-bottom: 5px;
  }
}

.header {
  position: sticky;
  padding: 10px;
  box-sizing: border-box;
  top: 0;
  left: 0;
  width: 100%;
  background: $primary;
  display: flex;
  align-items: center;
  border-bottom: 2px solid $dark;

  .home-link {
    margin-right: 20px;
  }

  h1 {
    color: white;
    font-size: 24px;
    margin: 0;
  }
}

footer {
  height: 100px;
  display: flex;
  align-items: center;
  padding: 0 20px;
  background: $primary;
}
</style>
