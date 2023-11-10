<script setup lang="ts">
import { Context } from '@needle-tools/engine';
import { onMounted, reactive, ref } from 'vue';

const props = defineProps<{
    backgroundColor?: string,
}>();

const self = ref<HTMLElement | null>(null);

onMounted(() => {

    // console.log("Slide mounted");
    // const slides = document.querySelectorAll(".slide *");
    // console.log(slides);
});

function nextSlide() {
    self.value?.dispatchEvent(new CustomEvent("next-slide"));
}

</script>

<template>
    <div ref="self" class="slide">
        <slot></slot>
        <button class="next" v-on:click="nextSlide">▶</button>
    </div>
</template>

<style scoped>
.slide {
    font-size: 20rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: v-bind(backgroundColor);
    /* opacity: 0;
    transition: opacity .1s ease-in-out; */
}
.slide .next {
    position: absolute;
    top: 50%;
    right: 3%;
    transform: translateY(-50%);
    font-size: 2.5rem;
    border-radius: 100%;
    width: 5rem;
    height: 5rem;
    line-height: 3rem;
    padding-left: .6rem;
    border: none;
    pointer-events: all;
    color: rgba(0, 0, 0, .2);
    background: rgba(255, 255, 255, .5);
    z-index: 999;
    cursor: pointer;
}

@media (max-width: 600px) {
    .slide {
        font-size: 7rem;
    }
    .slide .next {
        top: 8%;
    }
}


</style>