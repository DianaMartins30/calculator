<template>
    <div class="grid grid-cols-4 gap-2">
        <KeyBoardKey :key-prop="key" v-for="key in keys" :key="key.key" @key-pressed="handleKey($event)">
        </KeyBoardKey>
    </div>
</template>

<script>
import KeyBoardKey from './KeyBoardKey.vue'
export default {
    components: {
        KeyBoardKey
    },
    props: {
        keys: {
            type: Array,
            default() {
                return []
            }
        },
    },
    methods: {
        handleKey(key) {
            this.$emit("key-pressed", key)

        }
    },
}
</script>

<style lang="scss" scoped>
</style>