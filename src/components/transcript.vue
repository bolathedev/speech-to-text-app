<template>
    <p ref="word" :class="`word ${current ? 'active' : ''}`"> {{ word }} </p>
</template>

<script>
import { TimelineLite, Back } from 'gsap'
import scrollTrigger from 'gsap/ScrollTrigger'
import gsap from 'gsap'
// register the plugin
gsap.registerPlugin(scrollTrigger);
let wordTrigger = null
export default {
    name: 'Transcript',
    props: {
        word: {
            type: String,
            required: true
        },
        current: {
            type: Boolean,
            required: true,
            default: false
        },
    },
    mounted() {
        const { word } = this.$refs;

        wordTrigger = new TimelineLite({
            scrollTrigger: {
                trigger: '.word',
                scrub: 4,
            }
        });
        wordTrigger.from(word, 1, {
            opacity: 0,
            y: 100,
            ease: Back.easeOut,
        });
    }
}
</script>

<style>
p {
    color: white;
    margin: 1em;
    font-size: 34px
}

.active {
    color: red;
    font-size: 56px;
}
</style>