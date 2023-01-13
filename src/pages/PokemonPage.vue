<template>
    <h1 v-if="!pokemon">Cargando...</h1>

    <div v-else>
        <h1>¿Quién es este Pokémon?</h1>

        <PokemonPicture 
            :pokemonId="pokemon.id" 
            :showPokemon="showPokemon"
        />

        <PokemonOptions 
            :pokemons="pokemonArr" 
            @selection="checkAnswer"
        />

        <template v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            <button @click="newGame">
                Reiniciar juego 
            </button>
        </template>
    </div>
  

</template>

<script>

import PokemonPicture from '@/components/PokemonPicture'
import PokemonOptions from '@/components/PokemonOptions'

import getPokemonOptions from '@/helpers/getPokemonOptions'
// console.log(getPokemonOptions())

export default {
    components: { PokemonOptions, PokemonPicture },
    data() {
        return {
            pokemonArr : [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            massage: ''
        }
    },
    methods: {
        async mixPokemonArray(){
            this.pokemonArr = await getPokemonOptions()

            const rndInt = Math.floor(Math.random() * 4)
            this.pokemon = this.pokemonArr[rndInt]
        },
        checkAnswer(selectedId) {
            this.showPokemon = true
            this.showAnswer = true

            if(selectedId == this.pokemon.id){
                this.message = `Correcto, el Pokemon es ${this.pokemon.name}`
            }else{
                this.message = `¡Ups!, el Pokemon era ${this.pokemon.name} :(`
            }
        },
        newGame(){
            this.showPokemon = false
            this.showAnswer = false
            this.pokemonArr = []
            this.pokemon = null
            this.mixPokemonArray()

        }
    },
    mounted() {
        this.mixPokemonArray()
    }
}
</script>