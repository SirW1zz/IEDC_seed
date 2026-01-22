<script setup>
import { ref, onMounted } from 'vue';
import FramedMainSection from '@/layouts/FramedMainSection.vue'
import team from '@/data/team.json'
import RotateOnScroll from '@/components/RotateOnScroll.vue'
import TestimonialWriterCard from '@/components/TestimonialWriterCard.vue';

const frameRef = ref(null);
const frameSectionRef = ref(null);

onMounted(async () => {
    frameSectionRef.value = frameRef.value.sectionRef;
})
</script>

<template>
    <FramedMainSection ref="frameRef" id="team" class="min-h-[100dvh] flex relative">
        <RotateOnScroll v-if="frameSectionRef" :contentSection="frameSectionRef">
            <template #content="{ registerContainer }">
                <div class="h-full w-full bg-cover rounded-4xl text-center flex items-start relative">
                    <div :ref="registerContainer"
                        class="flex flex-col relative items-center w-fit z-30 perspective-distant text-black" style="height: auto;">
                        <div v-for="(member, i) in team" :key="i"
                            class="testimonial-item w-full text-left flex flex-col gap-2 py-8 px-20 max-md:px-6">
                            <TestimonialWriterCard 
                            :writerImage="member.writer_image" 
                            :writerPosition="member.writer_position" 
                            :starsCount="member.stars" 
                            />
                            <p class="font-rubik text-xl max-md:text-base max-md:leading-snug">
                                {{ member.description }}
                            </p>
                        </div>
                    </div>
                </div>
            </template>
            <template #background>
                <img src="/backgrounds/sky.jpg" alt="" class="h-full w-full"/>
            </template>
        </RotateOnScroll>
    </FramedMainSection>
</template>
