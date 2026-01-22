<script setup>
import FramedMainSection from '@/layouts/FramedMainSection.vue'
import stories from '@/data/about-me.json'
import { useScrollContext } from '@/composables/useScrollContext';
import { ref, onMounted, onBeforeUnmount } from 'vue';
import FollowingFrame from '@/components/FollowingFrame.vue';
import { AnimatedComponent } from '@/services/AnimatedComponent';

const component = ref(null)
const frameRef = ref(null)
const topRef = ref()
const frameSectionRef = ref(null)
const { containerRef } = useScrollContext()
const contentRef = ref(null)
const triggerSectionRef = ref(null)
const triggerSections = ref([])
const translationY = ref(0)
const sectionPercent = ref(0)
const currentSectionId = ref(0)


const prepareAnimation = () => {
    let triggerCount = 0;
    
    triggerSections.value.forEach((el) => {
        if (el.getBoundingClientRect().top <= 0) triggerCount++;
    });

    currentSectionId.value = Math.min(triggerCount, triggerSections.value.length - 1);
    const currentSection = triggerSections.value[currentSectionId.value];
    const currentScrollTop = currentSection.getBoundingClientRect().top
    const currentOffsetTop = currentSection.offsetTop

    if (triggerCount > triggerSections.value.length -1 ) {

         sectionPercent.value = 100;
    } else if (triggerCount <= triggerSections.value.length -1 && currentScrollTop <= currentOffsetTop) {

        const min = (triggerCount == 0) ? 0 : triggerSections.value[triggerCount - 1].offsetTop;
        const max = currentOffsetTop - min;
        sectionPercent.value = Math.min((max - currentScrollTop) * 100 / max, 100);
    } else if (triggerCount <= triggerSections.value.length -1 && currentScrollTop > currentOffsetTop) {

        sectionPercent.value = 0;
    }  

    translationY.value = -100 * currentSectionId.value;
}

const tick = () => {
    contentRef.value.style.transform = `translateY(${translationY.value}%)`
}

onMounted(async () => {
    frameSectionRef.value = frameRef.value.sectionRef;
    triggerSections.value = Array.from(triggerSectionRef.value.children);
    component.value = new AnimatedComponent(frameRef.value.sectionRef);
    component.value.prepareForAnimations = prepareAnimation;
    component.value.tick = tick;
    component.value.addAnimationTrigger(containerRef.value, "scroll")
})

onBeforeUnmount(() => {
    component.value.reset();
})


</script>

<template>
    <FramedMainSection ref="frameRef" id="about" class="min-h-[100dvh] flex items-center relative">
        <!-- Frame -->
        <FollowingFrame v-if="frameSectionRef" :contentSection="frameSectionRef">
            <div ref="topRef" class="h-full w-full bg-white p-[3dvw]">
                <div class="h-full w-full relative flex justify-center items-center rounded-4xl overflow-hidden">
                    <div class="absolute h-full w-full top-0 left-0 bg-[url('/backgrounds/room.jpg')] bg-cover bg-bottom"></div>
                    <div class="h-[75%] w-full overflow-hidden px-[5dvw] z-10">
                        
                        <!-- Navigation / Progress Bar -->
                        <div class="absolute top-[8%] left-[5dvw] h-6 z-30 flex items-center gap-4 text-white">
                            <span class="min-w-fit font-ledger text-sm opacity-80 uppercase tracking-widest">{{ stories[currentSectionId].from }}</span>
                            <div class="h-[2px] w-48 flex items-center bg-white/20">
                                <div class="h-full bg-white transition-all duration-300" :style="`width: ${sectionPercent}%`"></div>
                            </div>
                            <span class="min-w-fit font-ledger text-sm opacity-80 uppercase tracking-widest">{{ stories[currentSectionId].to }}</span>
                        </div>

                        <div ref="contentRef" class="h-full w-full transition-transform duration-400 text-white">
                            <div v-for="story in stories" :key="story.when" class="h-full w-full flex items-center justify-between gap-12">
                                <!-- Text Container: 55% width -->
                                <div class="w-[55%] flex flex-col gap-6 pt-16">
                                    <h1 class="font-bold text-3xl xl:text-4xl leading-tight font-rubik text-red-custom uppercase tracking-tighter">{{ story.when }}</h1>
                                    <p class="text-lg xl:text-xl font-ledger leading-relaxed opacity-95 overflow-y-auto max-h-[35dvh] pr-4" style="scrollbar-width: thin; scrollbar-color: white transparent;">
                                        {{ story.description }}
                                    </p>
                                </div>
                                <!-- Image Container: 40% width -->
                                <div class="w-[40%] h-full flex items-center justify-center p-4">
                                    <img :src="story.image" alt="" class="rounded-2xl w-full h-full object-cover shadow-2xl transition-transform hover:scale-[1.02] duration-500 border border-white/10"/>
                                </div>
                            </div>
                        </div>
                    </div>

                </div>
            </div>
        </FollowingFrame>

        <div ref="triggerSectionRef" class="w-full flex flex-col justify-between py-[40dvh]">
            <div v-for="story in [...stories]" class="h-[60dvh]"></div>
        </div>
    </FramedMainSection>
</template>
