<script setup>
import { ref, onMounted } from 'vue';
import TextAnim from '@/components/TextAnim.vue'
import FramedMainSection from '@/layouts/FramedMainSection.vue';

const frameRef = ref(null);
const containerRef = ref(null);

onMounted(() => {
    containerRef.value = frameRef.value.sectionRef;
});
</script>

<template>
    <FramedMainSection ref="frameRef" id="home" class="h-dvh">
        <div 
            class="relative h-full w-full overflow-hidden rounded-4xl bg-black flex flex-col justify-center items-center text-center">
            
            <!-- Video Background -->
            <video 
                autoplay 
                muted 
                loop 
                playsinline 
                class="absolute inset-0 w-full h-full object-cover opacity-60">
                <source src="/background.mp4" type="video/mp4">
            </video>

            <!-- Gradient Overlay for readability -->
            <div class="absolute inset-0 bg-gradient-to-b from-black/40 via-transparent to-black/40 pointer-events-none"></div>
            
            <div
                class="relative text-[#fff] font-rubik uppercase leading-none flex flex-col justify-center items-center pointer-events-none w-full">
                <div class="flex h-[12dvw] xl:h-[15dvh] overflow-hidden w-full justify-center">
                    <TextAnim 
                        v-if="containerRef"
                        :texts="['THINK.', 'INNOVATE.', 'INSPIRE.']" 
                        :container="containerRef" 
                        :duration="2.5" 
                        :class="'text-red-custom text-[12dvw] xl:text-[15dvh]'" 
                    />
                </div>
                <p class="text-[3dvw] xl:text-[4dvh] mt-4 font-ledger opacity-90 tracking-[0.2em]">WE ARE RSET IEDC.</p>
            </div>
        </div>
    </FramedMainSection>
</template>

<style scoped>
</style>
