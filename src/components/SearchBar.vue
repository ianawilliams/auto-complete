<template>
  <div class="search-bar" :class="{'search-bar--active': active}">
    <div class="search-bar__input">
      <label>
        <span class="material-icons search-bar__icon">search</span>
        <input
          :value="value" type="text" 
          :placeholder="placeHolder" 
          @input="updateVal" 
          @focus="activate(true)"
          @blur="activate(false)"
        />
      </label>
    </div>
    <template v-if="clearable">
      <button 
        class="btn--icon btn--clear" 
        :class="{'hidden': !showClear}" 
        @click="$emit('input', '')"
      >
        <span class="material-icons">close</span>
      </button>
    </template>
  </div>
</template>
<script>
export default {
  name: 'SearchBar',
  props: {
    value: {
      type: String,
    },
    placeHolder: {
      default: 'Search...',
      type: String
    },
    clearable: {
      default: true,
      type: Boolean
    },
    dropdownActive: {
      default: false,
      type: Boolean
    }
  },
  data() {
    return {
      active: false,
    }
  },
  computed: {
    showClear() {
      return this.value && this.value.length > 0;
    }
  },
  methods: {
    updateVal(e) {
      this.$emit('input', e.target.value)
    },
    activate(val) {
      this.active = val;
      this.$emit(val ? 'focus' : 'blur')
    }
  }
}
</script>
<style lang="scss" scoped>
@import "@/styles/scss/_variables.scss";
.search-bar,
.search-bar__input,
label,
button {
  display: flex;
  align-items: center;
}

.btn--clear {
  color: $grey;
}

.search-bar {
  position: relative;
  padding: 0px 5px;
  border: 1px solid $grey;
  height: 40px;
  border-radius: 20px;
  box-sizing: border-box;
  background: $background;

  &.search-bar--active {
    border-color: $primary;

    .search-bar__icon {
      color: $primary;
    }
  }

  .search-bar__icon {
    color: $grey;
  }

  .search-bar__input {
    flex-grow: 1;
  }

  label,
  input {
    width: 100%;
    border: none;
    outline: none;
  }

  button {
    cursor: pointer;
    padding: 0;
    border-radius: 100%;
  }
}
</style>
