<script setup lang="ts">
import { Animator, Context, findObjectOfType, getParam, onStart, setParamWithoutReload } from '@needle-tools/engine';
import { onMounted } from 'vue';

const slides: HTMLElement[] = [];
let activeSlideIndex = -1;
let context: Context | null = null;
let animator: Animator | null = null;

// onStart is a Needle Engine hook that is called once at the beginning of the first frame
onStart(context => {

    // get the animator at the start of the application once
    // further down when the active slide changes we update this animator's slide property to update the animation state
    animator = findObjectOfType(Animator);

    // we want to network the slide changes so that all clients see the same slide
    context.connection.beginListen("slide-changed", newSlide => {
        // check if the new slide is a number and not the current slide
        if (typeof newSlide === "number" && newSlide !== activeSlideIndex) {
            updateActiveSlide(newSlide);
        }
    });
});

onMounted(() => {
    // get our HTML slides
    const root = document.querySelector(".slidedeck") as HTMLElement;
    const content = root.querySelectorAll(".slide");
    // iterate over the slides and create a new slide element if the content is not a slide
    for (let i = 0; i < content.length; i++) {
        const slide = content[i];
        slide.addEventListener("next-slide", () => {
            updateActiveSlide(activeSlideIndex + 1, true);
        });
        slide.addEventListener("prev-slide", () => {
            updateActiveSlide(activeSlideIndex - 1, true);
        });
        if (slide instanceof HTMLElement) {
            if (slide.classList.contains("slide")) {
                slides.push(slide);
                continue;
            }
            const slideElement = document.createElement("div");
            slides.push(slideElement);
            slideElement.classList.add("slide");
            slideElement.appendChild(slide);
            root.appendChild(slideElement);
        }
    }

    // once on startup we set the slide index from the URL if the user just opened the website
    onSetSlideIndexFromUrl();

    // listen to window events to change the slide
    window.addEventListener("keyup", evt => {
        const key = evt.key.toLowerCase();
        let currentSlide = activeSlideIndex;
        switch (key) {
            case "a":
            case "arrowleft":
                currentSlide--;
                updateActiveSlide(--currentSlide, true);
                break;
            case "d":
            case "arrowright":
                updateActiveSlide(++currentSlide, true);
                break;
        }
    });
});

function onSetSlideIndexFromUrl() {
    const slide = getParam("slide");
    let index = 0;
    if (slide) {
        if (slide === "string")
            index = parseInt(slide);
        else if (typeof slide === "number")
            index = slide;
    }
    else index = 0;
    updateActiveSlide(index);
}

function updateActiveSlide(index: number, userAction: boolean = false) {
    if (index === activeSlideIndex) return;
    if (index < 0) {
        index = slides.length - 1;
    }
    else if (index >= slides.length) {
        index = 0;
    }
    activeSlideIndex = index;

    if (userAction) context?.connection.send("slide-changed", index);

    // set the needle engine animator integer to update the animation state
    animator?.setInteger("slide", index);

    // here you can notify e.g. a component in the 3d engine to e.g. load another scene
    setParamWithoutReload("slide", index.toString());

    for (let i = 0; i < slides.length; i++) {
        const slide = slides[i];
        const isActive = i === index;
        const isLeft = i > index;
        const isRight = i < index;
        if (isActive) {
            slide.setAttribute("active", "");
        }
        else {
            slide.removeAttribute("active");
        }
        if (isLeft) {
            slide.setAttribute("left", "");
        }
        else {
            slide.removeAttribute("left");
        }
        if (isRight) {
            slide.setAttribute("right", "");
        }
        else {
            slide.removeAttribute("right");
        }
    }
}
</script>

<template>
    <div class="slidedeck">
        <slot></slot>
    </div>
</template>

<style>
.slidedeck {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;

    display: flex;
    align-items: center;
    justify-content: center;
    pointer-events: none;
}

.slide {
    position: absolute;
    top: 0;
    width: 100%;
    height: 100%;
    transition: left .2s ease-in-out, opacity .01s ease-in;
    opacity: 1;
}

.slide[active] {
    left: 0;
    opacity: 1;
}

.slide[left] {
    left: 100%;
}

.slide[right] {
    left: -100%;
}
</style>
