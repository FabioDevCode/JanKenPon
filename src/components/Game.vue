<script setup lang="ts">
import { ref } from 'vue';
import type { Ref } from 'vue';

// Data
let player_choice: Ref<boolean> = ref(false);
let go_ahead: Ref<boolean> = ref(false);


// Methods
const toggleChoice = () => (
    setTimeout(() => {
        player_choice.value = !player_choice.value
    }, 300)
);

const play_game = () => (
    console.log('Play the game')
);



defineExpose({
    toggleChoice,
    play_game,
});
</script>

<template>
    <div id="game_zone">


        <button v-if="!player_choice" id="jouer" @click="toggleChoice()">JOUER</button>


        <transition name="slide-fade">
            <div v-if="player_choice" id="player_choice">
                <div id="choice_btns">
                    <button>PLAYER vs PLAYER</button>
                    <button>PLAYER vs CPU</button>
                </div>

                <button id="close_choice" @click="toggleChoice()">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M310.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L160 210.7 54.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L114.7 256 9.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L160 301.3 265.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L205.3 256 310.6 150.6z"/></svg>
                </button>
            </div>
        </transition>

    </div>
</template>

<style scoped>
    #game_zone {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        /* background-color: rgba(250, 250, 251, .7); */
        background-color: transparent;
        /* box-shadow: 0 0 12px 2px rgba(75, 82, 122, .2); */
        height: 600px;
        width: 100%;
        max-width: 800px;
        min-width: 320px;
        margin: 0 auto 20px auto;
        border-radius: 15px;
    }

    button {
        cursor: pointer;
        display: flex;
    }
    button:active {
        transform: scale(.98);
    }

    #jouer {
        font-family: 'Unbounded', Arial, Helvetica, sans-serif;
        font-size: 3em;
        padding: 15px 45px;
        margin: 0 auto;
        border-radius: 20px;
        border: 5px solid white;
        color: white;
        background: rgb(239,61,97);
        box-shadow: 0 0 12px 6px rgba(75, 82, 122, .5);
        animation: btn_play 1500ms ease-in-out infinite;
    }
    #jouer:hover {
        animation: none;
    }
    #jouer:active {
        transform: scale(.98);
        box-shadow: 0 0 12px 2px rgba(75, 82, 122, .5);
    }

    @keyframes btn_play {
        0% {
            transform: scale(1);
        }
        50% {
            transform: scale(1.05);
        }
        100% {
            transform: scale(1);
        }
    }

    #player_choice {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    #choice_btns {
        display: flex;
        flex-direction: column;
        margin-bottom: 30px;
        width: 250px;
    }

    #choice_btns button {
        font-weight: 600;
        font-size: 1.2em;
        padding: 20px 25px;
        border-radius: 15px;
        display: flex;
        justify-content: center;
        margin: 10px 0;
        color: white;
        box-shadow: 0 0 12px 2px rgba(75, 82, 122, .5);
        background-color: #EF3D61;
        border: 4px solid white;
    }

    #choice_btns button:nth-child(1) {
        font-style: italic;
        cursor: not-allowed;
    }
    #choice_btns button:nth-child(1):active {
        transform: scale(1);
    }

    #close_choice {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 50px;
        width: 50px;
        border-radius: 8px;
        background-color: #F6E56C;
        border: 4px solid #4B527A;
        box-shadow: 0 0 12px 2px rgba(250, 250, 250, .3);
        transform: rotate(45deg);
    }
    #close_choice:active {
        transform: rotate(45deg) scale(.96);
    }

    #close_choice svg {
        fill: #4B527A;
        height: 80%;
        width: 80%;
        transform: rotate(-45deg);
    }


    .slide-fade-enter-active {
        transition: all 0.1s ease-out;
    }
    .slide-fade-leave-active {
        transition: all 0.1s cubic-bezier(1, 0.5, 0.8, 1);
    }
    .slide-fade-enter-from,
    .slide-fade-leave-to {
        transform: translateY(20px);
        display: none;
    }


</style>