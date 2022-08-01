<template>
    <div @click="handleKey(keyProp)" class="flex justify-center items-center rounded w-20 h-8"
        :class="[keyColor(keyProp), textColor(keyProp)]">
        {{ keyProp.key }}
    </div>
</template>

<script>
export default {
    props: {
        keyProp: {
            type: Object,
            default() {
                return {}
            }
        },
    },
    methods: {
        handleKey(key) {
            this.$emit('key-pressed', key)
        },
        keyColor(key) {
            switch (key.color) {
                case 'light':
                    return 'bg-gray-100'
                case 'dark':
                    return 'bg-gray-300'
                case 'blue':
                    return 'bg-blue-500'
            }
        },
        textColor(key) {
            switch (key.color) {
                case 'light':
                case 'dark':
                    return 'text-gray-900'
                case 'blue':
                    return 'text-white font-bold'
            }
        },
    },
}
</script>

<style lang="scss" scoped>
</style>