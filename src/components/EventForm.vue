<template>
    <div id="event-form" :class="{active: active}" :style="{top: top, left: left}">
        <h4>Adde an event</h4>
        <button id="close-button" @click="close">&#10005;</button>
    </div>
</template>
<script>
export default {
    methods: {
        close() {
            this.$store.commit("eventFormActive", false);
        }
    },
    computed: {
        active() {
            return this.$store.state.eventFormActive;
        },
        top() {
            return `${this.$store.state.eventFormPosY}px`;
        },
        left() {
            return `${this.$store.state.eventFormPosX}px`;
        }
    }
};
</script>
