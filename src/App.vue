<script setup lang="ts">
import SlideDeck from "./components/SlideDeck.vue"
import Slide from "./components/Slide.vue"
import "@needle-tools/engine"
import "./generated/gen.js"
</script>

<template>
    <div class="root">
        <SlideDeck>
            <Slide background-color="transparent">
                <span class="title">Slide 1</span>
                <span class="subtitle">Use <strong>WASD</strong> or <strong>arrow keys</strong><br />to change slides</span>
            </Slide>
            <Slide background-color="#a4b43c">
                <span class="title">Slide 2</span>
                <span class="subtitle">Sample made with<br /><a href="https://needle.tools">Needle Engine</a> and
                    <a href="https://vuejs.org">Vue.js</a></span>
            </Slide>
            <Slide background-color="#ff8faa">
                <span class="title">Slide 3</span>
                <span class="subtitle">Find more samples <a href="https://engine.needle.tools/samples">here</a></span>
            </Slide>
        </SlideDeck>
        <!-- You can also put ^ this below or above the <needle-engine> component (make sure the Needle Engine camera has a transparent clearColor)-->
        <needle-engine camera-controls></needle-engine>
    </div>
</template>

<style scoped>
.root {
    position: absolute;
    display: block;
    width: 100%;
    height: 100%;
    background: #a8c291;
}

needle-engine {
    position: fixed;
    width: 100%;
    height: 100%;
}

.title {
    color: rgba(0, 0, 0, .2);
}

.subtitle {
    display: block;
    font-size: 4rem;
    text-align: center;
    z-index: 10;
    background: rgba(255, 255, 255, .2);
    backdrop-filter: blur(8px);
    border-radius: 1rem;
    padding: .5rem;
    box-shadow: 0 0 30px rgba(0, 0, 0, .2);
    pointer-events: all;
    ;
}

.subtitle a {
    text-decoration: none;
    color: inherit;
    background: rgba(0, 0, 0, .1);
    border-radius: 1rem;
    padding: 0 .7rem;
    cursor: pointer;
}

a:hover {
    cursor: pointer;
}
</style>