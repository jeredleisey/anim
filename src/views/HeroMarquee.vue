<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue';
import { gsap } from 'gsap';
import TweenTarget, { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const triggers = ScrollTrigger.getAll();

const message = ref("Have no fear of perfection, you'll never reach it.");

const gsapSet = () => {
  const sections: TweenTarget[] = gsap.utils.toArray('.letter');

  sections.forEach((section, i) => {
    gsap.to(section, {
      scrollTrigger: {
        scrub: true,
        toggleActions: 'play none none reverse',
        start: i * 300,
        end: (i + 5) * 300,
      },
      opacity: 1,
      textShadow: 'rgb(245, 245, 245) 0px 0px 1.7px',
    });
  });
};

onMounted(() => {
  ScrollTrigger.refresh();
  gsapSet();
});

onUnmounted(() => {
  triggers.forEach((trigger) => trigger.kill());
  ScrollTrigger.clearMatchMedia();
});
</script>

<template>
  <div>
    <div ref="el" class="outer">
      <section class="hero">
        <div class="animated-text">
          <span
            v-for="(letter, id) in message"
            class="letter"
            :id="id.toString()"
          >
            {{ letter }}
          </span>
        </div>
      </section>
    </div>
  </div>
</template>

<style>
body::-webkit-scrollbar {
  display: none;
}

.outer {
  background: url('../assets/film-grain-bg.jpeg') fixed;
  height: 1800vh;
}
.hero {
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
}
.animated-text {
  margin: 20px;
}

span {
  text-transform: uppercase;
  font-family: 'Righteous';
  font-size: 6vw;
  color: transparent;
  text-shadow: rgb(0, 0, 0) 0px 0px 3px;
  opacity: 0.1;
}
</style>
