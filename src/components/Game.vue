<script setup lang="ts">
import { ref } from 'vue';
import type { Ref } from 'vue';


// Data
let player_choice: Ref<boolean> = ref(false);
let go_ahead: Ref<boolean> = ref(false);
let result: Ref<boolean> = ref(false);

let choice: Ref<string | null> = ref(null);
let CPU_choice: Ref<string | null> = ref(null);
let result_text: Ref<string | null> = ref("Vous avez gagné !");

// Player
let p_pierre: Ref<boolean> = ref(false);
let p_papier: Ref<boolean> = ref(false);
let p_ciseaux: Ref<boolean> = ref(false);
// CPU
let c_pierre: Ref<boolean> = ref(false);
let c_papier: Ref<boolean> = ref(false);
let c_ciseaux: Ref<boolean> = ref(false);



const array_choice: [string] = ["pierre", "papier", "ciseaux"];


// Methods
const toggleChoice = () => (
    setTimeout(() => {
        player_choice.value = !player_choice.value
    }, 300)
);
const resetGame = () => (
    player_choice.value = false,
    go_ahead.value = false,
    choice.value = null,
    CPU_choice.value = null,
    result.value = false,
    result_text.value = null,

    p_pierre.value = false,
    p_papier.value = false,
    p_ciseaux.value = false
);
const play_game = () => (
    console.log('Play the game'),
    setTimeout(() => {
        player_choice.value = true,
        result.value = false,
        go_ahead.value = !go_ahead.value
    }, 200)
);
const choose = (choose: string) => (
    choice.value = choose,
    document.querySelectorAll('.janken')?.forEach(el => {
        el.classList.remove('active_choice')
    }),
    document.querySelector(`#${choose}`)?.classList.add("active_choice")
);

const resultGame = () => (
    function result() {
        const index = Math.floor(Math.random() * array_choice.length);
        CPU_choice.value = array_choice[index];

        switch(choice.value) {
            case 'pierre':
                p_pierre.value = true;
                p_papier.value = false;
                p_ciseaux.value = false;
                break;
            case 'papier':
                p_pierre.value = false;
                p_papier.value = true;
                p_ciseaux.value = false;
                break;
            case 'ciseaux':
                p_pierre.value = false;
                p_papier.value = false;
                p_ciseaux.value = true;
                break;
        }

        switch(CPU_choice.value) {
            case 'pierre':
                c_pierre.value = true;
                c_papier.value = false;
                c_ciseaux.value = false;
                break;
            case 'papier':
                c_pierre.value = false;
                c_papier.value = true;
                c_ciseaux.value = false;
                break;
            case 'ciseaux':
                c_pierre.value = false;
                c_papier.value = false;
                c_ciseaux.value = true;
                break;
        }

        if(choice.value === CPU_choice.value) {
            result_text.value = "Egalité !";
        };
        if((choice.value === "pierre" && CPU_choice.value === "ciseaux") || (choice.value === "papier" && CPU_choice.value === "pierre") || (choice.value === "ciseaux" && CPU_choice.value === "papier")) {
            result_text.value = "Victoire !";
        };
        if((CPU_choice.value === "pierre" && choice.value === "ciseaux") || (CPU_choice.value === "papier" && choice.value === "pierre") || (CPU_choice.value === "ciseaux" && choice.value === "papier")) {
            result_text.value = "Perdu !";
        };
    }(),
    player_choice.value = false,
    go_ahead.value = false,
    result.value = true
);



defineExpose({
    toggleChoice,
    resetGame,
    play_game,
    choose,
    resultGame
});
</script>

<template>
    <div id="game_zone">

        <button v-if="!player_choice && !go_ahead && !result" id="jouer" @click="toggleChoice()">JOUER</button>


        <div v-if="player_choice && !go_ahead && !result" id="player_choice">
            <div id="choice_btns">
                <!-- <button>PLAYER vs PLAYER</button> -->
                <button @click="play_game()"> PLAYER vs CPU </button>
            </div>

            <button id="close_choice" @click="toggleChoice()">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M310.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L160 210.7 54.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L114.7 256 9.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L160 301.3 265.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L205.3 256 310.6 150.6z"/></svg>
            </button>
        </div>

        <section v-if="go_ahead" id="go_ahead">
            <h2>
                <p>Faites votre choix</p>
                <span @click="resetGame()">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M310.6 150.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L160 210.7 54.6 105.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L114.7 256 9.4 361.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L160 301.3 265.4 406.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L205.3 256 310.6 150.6z"/></svg>
                </span>
            </h2>

            <div id="all_janken">
                <div id="pierre" class="janken" @click="choose('pierre')">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M144 0C117.5 0 96 21.5 96 48V96v28.5V176c0 8.8-7.2 16-16 16s-16-7.2-16-16V149.3l-9 7.5C40.4 169 32 187 32 206V244c0 38 16.9 74 46.1 98.3L128 384v96c0 17.7 14.3 32 32 32H320c17.7 0 32-14.3 32-32V374.7c46.9-19 80-65 80-118.7V176 160 144c0-26.5-21.5-48-48-48c-12.4 0-23.6 4.7-32.1 12.3C350 83.5 329.3 64 304 64c-12.4 0-23.6 4.7-32.1 12.3C270 51.5 249.3 32 224 32c-12.4 0-23.6 4.7-32.1 12.3C190 19.5 169.3 0 144 0z"/></svg>
                </div>
                <div id="papier" class="janken" @click="choose('papier')">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M288 32c0-17.7-14.3-32-32-32s-32 14.3-32 32V240c0 8.8-7.2 16-16 16s-16-7.2-16-16V64c0-17.7-14.3-32-32-32s-32 14.3-32 32V336c0 1.5 0 3.1 .1 4.6L67.6 283c-16-15.2-41.3-14.6-56.6 1.4s-14.6 41.3 1.4 56.6L124.8 448c43.1 41.1 100.4 64 160 64H304c97.2 0 176-78.8 176-176V128c0-17.7-14.3-32-32-32s-32 14.3-32 32V240c0 8.8-7.2 16-16 16s-16-7.2-16-16V64c0-17.7-14.3-32-32-32s-32 14.3-32 32V240c0 8.8-7.2 16-16 16s-16-7.2-16-16V32z"/></svg>
                </div>
                <div id="ciseaux" class="janken" @click="choose('ciseaux')">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M40 208c-22.1 0-40 17.9-40 40s17.9 40 40 40l180.2 0c-7.6 8.5-12.2 19.7-12.2 32c0 25.3 19.5 46 44.3 47.9c-7.7 8.5-12.3 19.8-12.3 32.1c0 26.5 21.5 48 48 48l32 0 64 0c70.7 0 128-57.3 128-128l0-113.1c0-40.2-16-78.8-44.4-107.3C444.8 76.8 413.9 64 381.7 64L336 64c-21.3 0-39.3 13.9-45.6 33.1l74.5 23.7c8.4 2.7 13.1 11.7 10.4 20.1s-11.7 13.1-20.1 10.4L288 129.9l0 .1L84 65.8C62.9 59.2 40.5 70.9 33.8 92s5.1 43.5 26.2 50.2L269.5 208 40 208z"/></svg>
                </div>
            </div>

            <button v-if="!choice" id="result_btn">RESULTAT</button>
            <button v-else id="result_btn" @click="resultGame()">RESULTAT</button>
        </section>

        <section v-if="result" id="resultat">
            <h2>
                {{ result_text }}
            </h2>

            <div id="fight">
                <div id="player_res" class="janken_bis">
                    <div v-if="p_pierre" id="p_pierre" class="player">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M144 0C117.5 0 96 21.5 96 48V96v28.5V176c0 8.8-7.2 16-16 16s-16-7.2-16-16V149.3l-9 7.5C40.4 169 32 187 32 206V244c0 38 16.9 74 46.1 98.3L128 384v96c0 17.7 14.3 32 32 32H320c17.7 0 32-14.3 32-32V374.7c46.9-19 80-65 80-118.7V176 160 144c0-26.5-21.5-48-48-48c-12.4 0-23.6 4.7-32.1 12.3C350 83.5 329.3 64 304 64c-12.4 0-23.6 4.7-32.1 12.3C270 51.5 249.3 32 224 32c-12.4 0-23.6 4.7-32.1 12.3C190 19.5 169.3 0 144 0z"/></svg>
                    </div>
                    <div v-if="p_papier" id="p_papier" class="player">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M288 32c0-17.7-14.3-32-32-32s-32 14.3-32 32V240c0 8.8-7.2 16-16 16s-16-7.2-16-16V64c0-17.7-14.3-32-32-32s-32 14.3-32 32V336c0 1.5 0 3.1 .1 4.6L67.6 283c-16-15.2-41.3-14.6-56.6 1.4s-14.6 41.3 1.4 56.6L124.8 448c43.1 41.1 100.4 64 160 64H304c97.2 0 176-78.8 176-176V128c0-17.7-14.3-32-32-32s-32 14.3-32 32V240c0 8.8-7.2 16-16 16s-16-7.2-16-16V64c0-17.7-14.3-32-32-32s-32 14.3-32 32V240c0 8.8-7.2 16-16 16s-16-7.2-16-16V32z"/></svg>
                    </div>
                    <div v-if="p_ciseaux" id="p_ciseaux" class="player">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M40 208c-22.1 0-40 17.9-40 40s17.9 40 40 40l180.2 0c-7.6 8.5-12.2 19.7-12.2 32c0 25.3 19.5 46 44.3 47.9c-7.7 8.5-12.3 19.8-12.3 32.1c0 26.5 21.5 48 48 48l32 0 64 0c70.7 0 128-57.3 128-128l0-113.1c0-40.2-16-78.8-44.4-107.3C444.8 76.8 413.9 64 381.7 64L336 64c-21.3 0-39.3 13.9-45.6 33.1l74.5 23.7c8.4 2.7 13.1 11.7 10.4 20.1s-11.7 13.1-20.1 10.4L288 129.9l0 .1L84 65.8C62.9 59.2 40.5 70.9 33.8 92s5.1 43.5 26.2 50.2L269.5 208 40 208z"/></svg>
                    </div>
                </div>

                <div id="CPU_res" class="janken_bis">
                    <div v-if="c_pierre" id="c_pierre" class="cpu">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M144 0C117.5 0 96 21.5 96 48V96v28.5V176c0 8.8-7.2 16-16 16s-16-7.2-16-16V149.3l-9 7.5C40.4 169 32 187 32 206V244c0 38 16.9 74 46.1 98.3L128 384v96c0 17.7 14.3 32 32 32H320c17.7 0 32-14.3 32-32V374.7c46.9-19 80-65 80-118.7V176 160 144c0-26.5-21.5-48-48-48c-12.4 0-23.6 4.7-32.1 12.3C350 83.5 329.3 64 304 64c-12.4 0-23.6 4.7-32.1 12.3C270 51.5 249.3 32 224 32c-12.4 0-23.6 4.7-32.1 12.3C190 19.5 169.3 0 144 0z"/></svg>
                    </div>
                    <div v-if="c_papier" id="c_papier" class="cpu">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M288 32c0-17.7-14.3-32-32-32s-32 14.3-32 32V240c0 8.8-7.2 16-16 16s-16-7.2-16-16V64c0-17.7-14.3-32-32-32s-32 14.3-32 32V336c0 1.5 0 3.1 .1 4.6L67.6 283c-16-15.2-41.3-14.6-56.6 1.4s-14.6 41.3 1.4 56.6L124.8 448c43.1 41.1 100.4 64 160 64H304c97.2 0 176-78.8 176-176V128c0-17.7-14.3-32-32-32s-32 14.3-32 32V240c0 8.8-7.2 16-16 16s-16-7.2-16-16V64c0-17.7-14.3-32-32-32s-32 14.3-32 32V240c0 8.8-7.2 16-16 16s-16-7.2-16-16V32z"/></svg>
                    </div>
                    <div v-if="c_ciseaux" id="c_ciseaux" class="cpu">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M40 208c-22.1 0-40 17.9-40 40s17.9 40 40 40l180.2 0c-7.6 8.5-12.2 19.7-12.2 32c0 25.3 19.5 46 44.3 47.9c-7.7 8.5-12.3 19.8-12.3 32.1c0 26.5 21.5 48 48 48l32 0 64 0c70.7 0 128-57.3 128-128l0-113.1c0-40.2-16-78.8-44.4-107.3C444.8 76.8 413.9 64 381.7 64L336 64c-21.3 0-39.3 13.9-45.6 33.1l74.5 23.7c8.4 2.7 13.1 11.7 10.4 20.1s-11.7 13.1-20.1 10.4L288 129.9l0 .1L84 65.8C62.9 59.2 40.5 70.9 33.8 92s5.1 43.5 26.2 50.2L269.5 208 40 208z"/></svg>
                    </div>
                </div>
            </div>

            <div id="btns">
                <button @click="play_game()">
                    REJOUER
                </button>
                <button @click="resetGame()">
                    RETOUR
                </button>
            </div>
        </section>
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
        margin: 30px auto 20px auto;
        border-radius: 15px;
        overflow: hidden;
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

    #go_ahead {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-around;
        height: 100%;
        width: 100%;
        padding: 20px;
    }

    #go_ahead h2 {
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 50px;
        width: 300px;
        padding: 0 10px;
        background-color: #FAFAFB;
        border-radius: 8px;
    }
    #go_ahead h2 span {
        cursor: pointer;
        height: 40px;
        width: 40px;
        display: flex;
        justify-content: flex-end;
        align-items: center;
    }
    #go_ahead h2 span svg {
        height: 80%;
        width: 80%;
    }
    #all_janken {
        display: flex;
    }
    #go_ahead .janken {
        cursor: pointer;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 150px;
        height: 150px;
        margin: 0 40px;
        border-radius: 15px;
        background-color: white;
        box-shadow: 0 0 12px 6px rgba(250, 250, 250, .4);
    }

    #go_ahead .janken:active {
        transform: scale(.98) rotate(45deg);
        border-radius: 45px;
    }

    #go_ahead .janken svg {
        height: 80px;
        width: 80px;
    }

    #go_ahead .janken:nth-child(1) {
        border: 8px solid #EF3D61;
    }
    #go_ahead .janken:nth-child(1) svg {
        transform: rotate(45deg);
        fill: #EF3D61;
    }
    #go_ahead .janken:nth-child(2) {
        border: 8px solid #565C82;
    }
    #go_ahead .janken:nth-child(2) svg {
        transform: rotate(45deg);
        fill: #565C82;
    }
    #go_ahead .janken:nth-child(3) {
        border: 8px solid #F6E56C;
    }
    #go_ahead .janken:nth-child(3) svg {
       transform: rotate(135deg) rotateX(180deg);
       fill: #F6E56C;

    }

    #go_ahead .active_choice {
        transform: rotate(45deg);
        border-radius: 45px;
    }

    #result_btn {
        font-size: 1.8em;
        font-weight: 600;
        height: 70px;
        padding: 0 35px;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 12px;
        color: white;
        background-color: #EF3D61;
        border: 5px solid white;
        box-shadow: 0 0 12px 2px rgba(75, 82, 122, .5);
    }

    #resultat {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-evenly;
        padding: 10px;
        height: 100%;
        width: 100%;
    }

    #resultat h2 {
        background-color: #FAFAFB;
        color: #565C82;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 25px 35px 20px 35px;
        border-radius: 15px;
        font-size: 2.6em;
    }

    #fight {
        display: flex;
        justify-content: space-evenly;
        align-items: center;
        height: 200px;
        width: 100%;
    }

    #fight .janken_bis {
        display: flex;
        justify-content: center;
        align-items: center;
        overflow: hidden;
        height: 150px;
        width: 150px;
        background-color: green;
        border-radius: 45px;
        background-color: white;
        box-shadow: 0 0 12px 6px rgba(250, 250, 250, .8);
        transform: rotate(45deg);
    }

    #fight .janken_bis svg {
        height: 70%;
        width: 70%;

    }

    .player, .cpu {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;
        width: 100%;
    }

    #p_pierre, #p_papier, #p_ciseaux, #c_pierre, #c_papier, #c_ciseaux {
        border-radius: 45px;
    }

    #p_pierre, #c_pierre  {
        border: 8px solid #EF3D61;
    }
    #p_papier, #c_papier {
        border: 8px solid #565C82;
    }
    #p_ciseaux, #c_ciseaux {
        border: 8px solid #F6E56C;
    }

    #p_pierre svg {
        transform: rotate(45deg);
        fill: #EF3D61;
    }
    #p_papier svg {
        transform: rotate(45deg);
        fill: #565C82;
    }
    #p_ciseaux svg {
        transform: rotateY(180deg) rotate(45deg);
        fill: #F6E56C;
    }



    #c_pierre svg {
        transform: rotateY(180deg) rotate(135deg);
        fill: #EF3D61;
    }
    #c_papier svg {
        transform: rotateY(180deg) rotate(135deg);
        fill: #565C82;
    }
    #c_ciseaux svg {
        transform: rotate(-45deg);
        fill: #F6E56C;
    }

    #btns {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-evenly;
        height: 150px;
    }

    #btns button:nth-child(1) {
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 1.5em;
        height: 60px;
        width: 170px;
        border-radius: 12px;
        color: white;
        background-color: #EF3D61;
        border: 5px solid white;
        box-shadow: 0 0 12px 2px rgba(75, 82, 122, .5);
    }

    #btns button:nth-child(2) {
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 1.5em;
        height: 60px;
        width: 170px;
        border-radius: 12px;
        color: #EF3D61;
        background-color: rgba(250, 250, 250, .7);
        border: 5px solid white;
        box-shadow: 0 0 12px 2px rgba(75, 82, 122, .5);
    }
</style>