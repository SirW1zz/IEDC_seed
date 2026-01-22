<script setup>
import { ref } from 'vue';
import FramedMainSection from '@/layouts/FramedMainSection.vue';
import contact from '@/data/contact.json'
import CustomA from '@/components/CustomA.vue';
import Parallax from '@/components/Parallax.vue';

const frameRef = ref(null)
const contactSectionRef = ref(null)
</script>

<template>
    <FramedMainSection ref="frameRef" id="contact" class="h-dvh">
        <Parallax>
            <div ref="contactSectionRef" class="h-full w-full relative rounded-4xl flex flex-col pt-24">
                <div class="absolute h-full w-full top-0 left-0 rounded-4xl bg-neutral-900 -z-10 overflow-hidden">
                    <div class="h-full w-full opacity-30 bg-[radial-gradient(circle_at_50%_50%,_rgba(222,46,46,0.2)_0%,_transparent_70%)]"></div>
                    <div class="absolute inset-0 bg-[url('/contact/sky.jpeg')] opacity-10 mix-blend-overlay bg-cover"></div>
                </div>

                <div class="flex flex-col flex-grow justify-around px-8 py-12">
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 w-full gap-12 text-white">
                        <div v-for="(value, key) in contact" :key="key" class="flex flex-col items-center md:items-start text-center md:text-left">
                            <h2 class="font-rubik border-b-2 border-dashed border-neutral-600 text-xl w-full md:w-fit pb-1">{{ key }}</h2>
                            <ul class="pt-6 text-2xl flex flex-col gap-3">
                                <li v-for="(h, index) in value" :key="index" class="w-fit mx-auto md:mx-0">
                                    <CustomA v-if="!h.hidden" :text="h.text" :href="h.href" :target="h.target" />
                                </li>
                            </ul>
                        </div>
                    </div>

                    <div class="flex flex-col items-center justify-center py-12">
                        <div class="flex flex-wrap justify-center items-center gap-16 md:gap-32">
                            <img src="/logos/rset.png" alt="RSET Logo" class="h-20 md:h-24 w-auto object-contain brightness-0 invert opacity-70 hover:opacity-100 transition-opacity" />
                            <img src="/logos/iedc.png" alt="IEDC Logo" class="h-20 md:h-24 w-auto object-contain brightness-0 invert opacity-70 hover:opacity-100 transition-opacity" />
                            <img src="/logos/rset_ innovation.png" alt="IIC Logo" class="h-20 md:h-24 w-auto object-contain brightness-0 invert opacity-70 hover:opacity-100 transition-opacity" />
                        </div>
                    </div>

                    <div class="w-full max-w-2xl mx-auto flex flex-col items-center pt-8">
                        <h3 class="text-white font-ledger text-2xl mb-6 opacity-80">Shoot us a question</h3>
                        <div class="flex w-full bg-white/5 border border-white/20 rounded-full p-2 focus-within:border-red-custom focus-within:bg-white/10 transition-all duration-300">
                            <input 
                                type="text" 
                                placeholder="Write your message here..." 
                                class="bg-transparent border-none outline-none text-white px-8 py-3 flex-grow font-ledger text-lg"
                            />
                            <button class="bg-red-custom text-white px-10 py-3 rounded-full font-rubik text-sm tracking-widest hover:bg-red-700 hover:scale-[1.02] active:scale-95 transition-all">
                                SEND
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </Parallax>
    </FramedMainSection>
</template>
