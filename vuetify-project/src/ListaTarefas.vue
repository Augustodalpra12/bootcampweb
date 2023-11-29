<script>
import CampoDeInput from './components/CampoDeInput.vue';
import Tarefa from './components/Tarefa.vue';
import  axios  from 'axios';

export default {
    components: {
        CampoDeInput,
        Tarefa,
    },
    data() {
        return {
            tarefas: [],
        };
    },
    methods: {
        async adicionarTarefa(novaTarefa) {
            if (novaTarefa.trim() !== '') {
                const newPokemon = await this.getPokemon(novaTarefa)
                console.log(newPokemon)
                this.tarefas.push({ nome: newPokemon.name, img: newPokemon.sprites.front_default });
            }
        },

        async getPokemon(pokemonName) {
            return axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`).then((response) => {
                return response.data;
            }).catch((error) => {
                console.error(error);
            });
        },

        atualizarConclusao(index, concluida) {
            this.tarefas[index].concluida = concluida;
        },
        removerTarefa(index) {
            this.tarefas.splice(index, 1);
        },
    },
};
</script>

<template>
    <div class="body">
        <h2 class="titulo">Lista de Tarefas</h2>
        <campo-de-input class="input" @tarefa-adicionada="adicionarTarefa"></campo-de-input>
        <div class="tarefas">
            <tarefa v-for="(tarefa, index) in tarefas" :key="index" :nome="tarefa.nome" :img="tarefa.img"
                @tarefa-concluida="atualizarConclusao(index, $event)" @remover-tarefa="removerTarefa(index)"></tarefa>
        </div>
    </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
</style>
<style scoped>
    .body {
        display: flex;
        flex-direction: column;
        align-items: center;
        
    }
    .titulo{
        font-family:"Press Start 2P";
    }
    
    .input {
        width: 30%;
    }

    

    .tarefas {
        margin-top: 20px;
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 100%;
    }
</style>