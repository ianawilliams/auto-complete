<template>
    <div>
        <div v-if="loading" class="result-list__msg">
            <div class="lds-ellipsis">
                <div></div>
                <div></div>
                <div></div>
                <div></div>
            </div>
        </div>
        <template v-else-if="list.length > 0">
            <ResultItem 
              v-for="(item, index) in list" 
              :key="index" :item="item"
              @select-item="$emit('select-item', item)"
            />
        </template>
        <div v-else class="result-list__msg">
            {{ emptyMsg }}
        </div>
    </div>
</template>
<script>
import ResultItem from "@/components/ResultItem.vue"

export default {
    name: "ResultList",
    components: {
        ResultItem
    },
    props: {
        emptyMsg: {
            default: 'No Results',
            type: String
        },
        searchTerm: {
            type: String,
        },
        loading: {
            type: Boolean,
            default: false,
        },
        list: {
            type: Array,
            default: () => [],
        },
    },
}
</script>
<style lang="scss" scoped>
@import "@/styles/scss/_variables.scss";

.result-list__msg {
    text-align: center;

    .lds-ellipsis {
  /* change color here */
  color: #1c4c5b
}
.lds-ellipsis,
.lds-ellipsis div {
  box-sizing: border-box;
}
.lds-ellipsis {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 13.33333px;
}
.lds-ellipsis div {
  position: absolute;
  // top: 33.33333px;
  width: 13.33333px;
  height: 13.33333px;
  border-radius: 50%;
  background: $primary;
  animation-timing-function: cubic-bezier(0, 1, 1, 0);
}
.lds-ellipsis div:nth-child(1) {
  left: 8px;
  animation: lds-ellipsis1 0.6s infinite;
}
.lds-ellipsis div:nth-child(2) {
  left: 8px;
  animation: lds-ellipsis2 0.6s infinite;
}
.lds-ellipsis div:nth-child(3) {
  left: 32px;
  animation: lds-ellipsis2 0.6s infinite;
}
.lds-ellipsis div:nth-child(4) {
  left: 56px;
  animation: lds-ellipsis3 0.6s infinite;
}
@keyframes lds-ellipsis1 {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes lds-ellipsis3 {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}
@keyframes lds-ellipsis2 {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(24px, 0);
  }
}
}
</style>