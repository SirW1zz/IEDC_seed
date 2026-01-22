<script setup>
import FramedMainSection from '@/layouts/FramedMainSection.vue';
import awards from '@/data/awards.json';
import Parallax from '@/components/Parallax.vue';
import { ref, onMounted } from 'vue';

const memberCount = ref(0);
const eventCount = ref(0);
const awardsRef = ref(null);
const animated = ref(false);

const animate = () => {
    if (animated.value) return;
    animated.value = true;
    
    const duration = 2000; // 2 seconds
    const startTime = performance.now();
    
    const update = (currentTime) => {
        const elapsed = currentTime - startTime;
        const progress = Math.min(elapsed / duration, 1);
        
        // Easing function (easeOutQuad)
        const easeProgress = 1 - (1 - progress) * (1 - progress);
        
        memberCount.value = Math.floor(easeProgress * 3000);
        eventCount.value = Math.floor(easeProgress * 200);
        
        if (progress < 1) {
            requestAnimationFrame(update);
        }
    };
    
    requestAnimationFrame(update);
};

onMounted(() => {
    const observer = new IntersectionObserver((entries) => {
        if (entries[0].isIntersecting) {
            animate();
        }
    }, { threshold: 0.1 });
    
    if (awardsRef.value) {
        observer.observe(awardsRef.value);
    }
});
</script>

<template>
     <FramedMainSection id="awards" class="h-dvh">
        <Parallax>
        <div ref="awardsRef" class="h-full w-full relative overflow-hidden rounded-4xl text-white flex justify-center items-start md:items-end text-center p-4">
            <div class="absolute h-full w-full bg-[url('/awards/sky.jpg')] bg-cover top-0 left-0 -z-10 overflow-visible">
                <div data-parallax-value=".02" class="parallax absolute top-2/5 left-1/2 -translate-x-1/2 -translate-y-1/2 aspect-video w-[130dvw] bg-center bg-cover bg-[url('/awards/clouds.png')]"></div>
                <img src="/awards/trophy.png" alt="" data-parallax-value=".15" class="parallax w-full absolute scale-200 -bottom-1/12 sm:scale-125 md:scale-150 lg:scale-125 lg:-bottom-1/3">
                
                <!-- New Facts -->
                <div data-parallax-value=".1" class="parallax max-md:!data-[parallax-value]:content-['.03'] absolute top-[35%] max-md:top-auto max-md:bottom-[15%] left-[5%] md:left-[15%] text-left flex flex-col pointer-events-none">
                    <span class="font-rubik text-3xl md:text-5xl lg:text-6xl max-md:text-3xl text-red-custom max-md:bg-gradient-to-r max-md:from-green-600 max-md:to-orange-500 max-md:bg-clip-text max-md:text-transparent">{{ memberCount }}+</span>
                    <span class="font-rubik text-xs md:text-sm lg:text-base max-md:text-xs opacity-80 uppercase font-bold max-md:text-white">Members</span>
                </div>
                <div data-parallax-value=".1" class="parallax max-md:!data-[parallax-value]:content-['.03'] absolute top-[35%] max-md:top-auto max-md:bottom-[15%] right-[5%] md:right-[15%] text-right flex flex-col pointer-events-none">
                    <span class="font-rubik text-3xl md:text-5xl lg:text-6xl max-md:text-3xl text-red-custom max-md:bg-gradient-to-l max-md:from-green-600 max-md:to-orange-500 max-md:bg-clip-text max-md:text-transparent">{{ eventCount }}+</span>
                    <span class="font-rubik text-xs md:text-sm lg:text-base max-md:text-xs opacity-80 uppercase font-bold max-md:text-white">Events</span>
                </div>
            </div>
            <div class="flex flex-col md:flex-row">
                <div v-for="award in awards" class="h-fit flex-1 p-4 flex flex-col items-start cursor-default text-left">
                    <span class="font-rubik">{{award.date}}</span>
                    <h1 class="font-rubik text-xl italic">{{award.title}}</h1>
                    <p>{{award.description}}</p>
                </div>
            </div>
        </div>
    </Parallax>
    </FramedMainSection>
</template>
