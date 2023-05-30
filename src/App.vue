<script setup>

import { ref, onMounted } from "vue"
import Blinding_Lights from "./assets/sound/Blinding_Lights.mp3"

const minute = ref(0)
const end = ref(10)
const pause = ref(false)
const timer = ref(null)
const typeTime = ref("work")

const start = function () {
    if (!pause.value) {
        pause.value = !pause.value
        timer.value = setInterval(() => {
            if (minute.value === 0 && end.value === 0) {
                pause.value = !pause.value
                if(typeTime.value === "work"){
                    minute.value = 3
                    end.value = 20
                    typeTime.value = "break"
                    BlindLights()
                    clearInterval(timer.value)
                    setTimeout(() => {
                        start()
                    }, 2000)
                } else {
                    minute.value = 0
                    end.value = 10
                    typeTime.value = "work"
                    clearInterval(timer.value)
                    start()
                }
            } else {
                end.value = end.value - 1
            }
            if (minute.value === 0 && end.value === -1) {
                end.value = end.value - 1
                pause.value = !pause.value
                if(typeTime.value === "work"){
                    minute.value = 0
                    end.value = 5
                    typeTime.value = "break"
                    BlindLights()
                    clearInterval(timer.value)
                    start()
                } else {
                    minute.value = 0
                    end.value = 10
                    typeTime.value = "work"
                    clearInterval(timer.value)
                    start()
                }
            }
            if (end.value === -1) {
                end.value = 59;
                minute.value = minute.value - 1
            }
        }, 1000)
    } else{
        clearInterval(timer.value)
        pause.value = !pause.value
    }
}

const BlindLights = () => {
    let breakMusic = document.querySelector("#BlindLights")
    breakMusic.play()
}


</script>


<template>
    <div class="wrapper w-full min-h-screen bg-[#F2FFF5]">
    <audio :src="Blinding_Lights" id="BlindLights" controls class="w-0 h-0 overflow-hidden"></audio>
        <div class="container mx-auto grid place-items-center h-screen">
            <div class="timer">
                <div class="status text-[24px]"><i class='bx bx-brain'></i> STATUS</div>
                <div class="numbers">
                    <h1 :class="isDark ? 'dark' : ''" class="text-center text-[200px] md:text-[256px] leading-[170px] md:leading-[217px]" v-if="minute < 10">0{{ minute }}</h1>
                    <h1 :class="isDark ? 'dark' : ''" class="text-center text-[200px] md:text-[256px] leading-[170px] md:leading-[217px]" v-else> {{ minute }}</h1>
                    <h1 :class="isDark ? 'dark' : ''" class="text-center text-[200px] md:text-[256px] leading-[170px] md:leading-[217px]" v-if="end < 10">0{{ end }}</h1>
                    <h1 :class="isDark ? 'dark' : ''" class="text-center text-[200px] md:text-[256px] leading-[170px] md:leading-[217px]" v-else>{{ end }}</h1>
                </div>
                <div class="controls gap-3 flex text-4xl">
                    <button class="p-[24px] rounded-[24px] duration-150 flex justify-center items-center bg-[#e4f9e9]"><i
                            class='bx bx-dots-horizontal-rounded'></i></button>
                    <button @click="start"
                        class="p-[24px] rounded-[24px] duration-150 flex justify-center items-center bg-[#e4f9e9]"><i
                            v-if="!pause" class='bx bx-play'></i><i v-else-if="pause" class='bx bx-pause'></i></button>
                    <button class="p-[24px] rounded-[24px] duration-150 flex justify-center items-center bg-[#e4f9e9]"><i
                            class='bx bx-skip-next'></i></button>
                    <!-- <span><i class='bx bx-dots-horizontal-rounded' ></i></span> <span><i class='bx bx-play'></i></span> <span><i class='bx bx-skip-next' ></i></span> -->
                </div>
            </div>

        </div>

    </div>
</template>


<style lang="scss" scoped>
.numbers h1 {
/*     font-size: 256px; */
/*     line-height: 217px; */
    font-weight: 800;
    color: #14401D
}

.numbers {
    margin: 32px 0;
}

.numbers h1 {
    font-family: 'Orbitron', sans-serif;
}

.status {
    padding: 8px 16px;
    border: 2px solid #14401D;
    border-radius: 48px;
    display: inline-block;
    gap: 20px;
    background-color: rgba(77, 218, 110, 0.15);
}

.timer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.controls button:focus {
    padding: 32px 48px;
    background-color: rgba(77, 218, 110, 0.62);
}

.controls button:hover {
    background-color: #86e299;
}

.controls {
    height: 120px;
    display: flex;
    align-items: center;
}
</style>