<template>
    <div
      class="auto-complete"
      :class="{
        'auto-complete--focus': inputActive,
        'auto-complete--active': showDropdown,
        'auto-complete--absolute': absolute,
        'auto-complete--relative': !absolute,
      }"
    >
        <SearchBar
          v-model="value" 
          :clearable="clearable"
          :place-holder="placeHolder"
          :dropdownActive="showDropdown"
          @focus="searchActive = true" 
          @blur="searchActive = false"
        />
        <div v-if="showDropdown" class="auto-complete__dropdown" :style="style">
          <slot>
            <ResultsList :list="list" :loading="animation" @select-item="selectItem"/>
          </slot>
        </div>
    </div>
</template>
<script>
import SearchBar from '@/components/SearchBar.vue'
import ResultsList from '@/components/ResultList.vue'
import debounce from 'lodash/debounce';

export default {
  name: 'AutoComplete',
  components: {
    SearchBar,
    ResultsList
  },
  props: {
    placeHolder: {
      default: 'Search...',
      type: String
    },
    clearable: {
      default: true,
      type: Boolean
    },
    maxHeight: {
      type: String
    },
    absolute: {
        type: Boolean,
        default: false,
    },
    suggestAll: {
        type: Boolean,
        default: false,
    },
    options: {
        type: Array,
        default: () => [],
    },
    disableFilter: {
      type: Boolean,
      default: false,
    },
    overRideResults: {
        type: Array,
        default: () => [],
    },
    loading: {
      type: Boolean,
      default: false,
    },

  },
  async mounted() {
    this.initClickEvent();
    this.results = await this.getResults("", this.options);
  },
  data() {
    return {
      value: "",
      searchActive: false,
      inputActive: false,
      results: [],
      filtering: false,
    }
  },
  computed: {
    style() {
      if (this.maxHeight) {
        return `max-height: ${this.maxHeight}`;
      } else {
        return "";
      }
    },
    hasValue() {
      return this.value && this.value.length > 0;
    },
    showDropdown() {
      if (this.inputActive) {
        if (this.suggestAll) {
          return true;
        } else {
          return this.hasValue;
        }
      } else {
        return false;
      }
    },
    list() {
      if (this.disableFilter) {
        return this.overRideResults;
      } else {
        return this.results;
      }
    },
    animation() {
      if (this.disableFilter) {
        return this.loading;
      } else {
        return this.filtering;
      }
    }
  },
  methods: {
    selectItem(val) {
      this.value = val;
      this.blurInput();

    },
    initClickEvent() {
      this.$el?.addEventListener("mousedown", this.handleInputClick);
    },
    initDocEvent() {
      document.addEventListener("mousedown", this.handleDocumentClick);
    },
    handleInputClick() {
      this.inputActive = true;
      this.initDocEvent();
      this.$el?.removeEventListener("mousedown", this.handleInputClick);
    },
    handleDocumentClick(e) {
      if (this.inputActive) {
        if (!this.$el?.contains(e.target)) {
          this.blurInput();    
        }
      }
    },
    blurInput() {
      this.inputActive = false;
          document.removeEventListener("mousedown", this.handleDocumentClick);
          this.initClickEvent();
    },
    
    async getResults(searchTerm, options) {
      if (this.hasValue) {
        return this.options.filter((item) => {
          return item.toLowerCase().includes(this.value.toLowerCase());
        })
      } else if (this.suggestAll) {
        return this.options;
      } else {
        return [];
      }
    },

    searchEvent: debounce(async function(val) {
      this.results = await this.getResults(val, this.options);
      this.filtering = false;
    }, 0)
  },
  watch: {
    value: function(newVal) {
      if (!this.disableFilter) {
        this.filtering = true;
        this.searchEvent(newVal);
      } else {
        this.$emit("search-term", newVal);
      }
    }
  }
}
</script>
<style lang="scss" scoped>
@import "@/styles/scss/_variables.scss";

.auto-complete {
  position: relative;

  &.auto-complete--absolute {
    .auto-complete__dropdown {
      position: absolute;
      width: 100%;
      z-index: 2;
      box-sizing: border-box;
    }
  }

  &.auto-complete--relative {
    border-bottom-right-radius: 20px;
    border-bottom-left-radius: 20px;
  }

  &.auto-complete--active {
    background: $background;
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
  }
  .auto-complete__dropdown {
    max-height: 300px;
    overflow: auto;
    background: $background;
    padding: 10px 0;
    box-shadow: 0px 8px 10px 1px rgba(0, 0, 0, 0.25);
    padding-bottom: 10px;
    border-bottom-right-radius: 20px;
    border-bottom-left-radius: 20px;
  }
}
</style>
