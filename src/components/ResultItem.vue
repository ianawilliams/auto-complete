<template>
    <div>
        <button class="btn-result btn--unstyled" @click="clickHandle">
            {{ item }}
        </button>
    </div>
</template>
<script>
export default {
    name: "ResultList",
    props: {
        item: {
            type: String
        },
        searchTerm: {
            type: String
        }
    },
    methods: {
        clickHandle() {
            console.log(`Clicked: ${this.item}`)
            this.$emit('select-item', this.item);
        }
    }
}
</script>
<style lang="scss" scoped>
@import "@/styles/scss/_variables.scss";

.btn-result {
    font-size: 14px;
    display: block;
    width: 100%;
    text-align: left;
    padding: 5px 10px;
    cursor: pointer;

    &:hover {
        background: $primary;
        color: #FFF;
    }
}
</style>