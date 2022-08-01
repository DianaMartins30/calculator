<template>
    <input type="number" :value="value" @input="$emit('update: value', $event.target.value)">
</template>

<script>
export default {
    name: 'InputField',

    props: {
        value: {
            default: 0
        }
    }


}
</script>
