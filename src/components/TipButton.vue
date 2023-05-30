<template>
    <button :class="value===active?'tipButtonActive':''" @click.prevent="updateActiveTip">{{ value }}%</button>
</template>
<script>
export default {
    props: ['value', 'active'],
    methods: {
        updateActiveTip(){
            this.$emit('onUpdateTip', this.value)
        }
    }
}
</script>
<style scoped>

button {
    transition: background-color 0.2s ease-out 0s;
    @apply
        lg:h-14
        bg-very-dark-cyan
        text-white
        text-2xl
        py-2
        rounded-md
        hover:bg-[rgba(159,232,223,255)]
        hover:text-very-dark-cyan
}

button:active {
    transition: background-color 0s ease-in 0s;
        @apply
            active:bg-very-light-grayish-cyan
}

.tipButtonActive {
    @apply
        bg-strong-cyan
        text-very-dark-cyan
}

</style>