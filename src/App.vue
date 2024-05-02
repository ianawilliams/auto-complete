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
            removing the loading animation.
        </p>
      </div>
    </main>
  </div>
</template>

<script>
import AutoCompleteVue from '@/components/AutoComplete.vue'

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
    async apiMock(searchTerm) {
      this.apiResults = this.list.filter((item) => {
        return item.toLocaleLowerCase().includes(searchTerm.toLocaleLowerCase());
      })
    },
    async loadResults(val) {
      if (val) {
        this.loading = true;
        await this.apiMock(val);
        setTimeout(() => {
          this.loading = false;
        }, "1500");
      }
    }
  }
}
</script>

<style lang="scss">
@import 'https://fonts.googleapis.com/icon?family=Material+Icons';
@import '@/styles/scss/index.scss';

body {
  margin: 0;
  background: #eaeaea;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

main {
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
    text-decoration: none;
    color: #FFF;
    margin-right: 20px;

    span {
      display: flex;
    }
  }

  h1 {
    color: white;
    font-size: 24px;
    margin: 0;
  }
}
</style>
