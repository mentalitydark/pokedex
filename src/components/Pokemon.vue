<template>
  <div class="pokemon">
    <div class="card">
        <div class="card-image">
            <figure >
                <img :src="pokemon.sprite" alt="Placeholder image">
            </figure>
        </div>
        <div class="card-content">
                <div class="media">
                    <div class="media-content">
                        <p class="title is-4">{{nome | FirstUpper}}</p>
                        <div v-if="(pokemon.type.slot === 2)">
                            <p class="subtitle is-6">{{pokemon.type.Um}}</p>
                            <p class="subtitle is-6">{{pokemon.type.Dois}}</p>
                        </div>
                        <div v-else>
                            <p class="subtitle is-6">{{pokemon.type.Um}}</p>
                        </div>
                    </div>
                </div>
            <div class="content">
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
    data(){
        return {
            pokemon: {
                type: {
                    slot: 0,
                    Um: '',
                    Dois: ''
                },
                sprite: ''
            }
        }
    },
    props: {
        nome: String,
        url: String,
        number: Number
    },
    filters: {
        FirstUpper: function(value) {
            let nameTemp = value[0].toUpperCase() + value.slice(1);
            return nameTemp
        }
    },
    created: function() {
        axios.get(this.url).then(res => {
            // this.pokemon.type = res.data.types[0].type.name;
            if (res.data.types[1] !== undefined) {
                this.pokemon.type.slot = 2;
                this.pokemon.type.Um = res.data.types[0].type.name;
                this.pokemon.type.Dois = res.data.types[1].type.name;
            } else {
                this.pokemon.type.slot = 1;
                this.pokemon.type.Um = res.data.types[0].type.name;
                this.pokemon.type.Dois = undefined;
            }
            this.pokemon.sprite = res.data.sprites.other["official-artwork"].front_default;
            console.log("Requisição Feita com sucesso.");
        }).catch(erro => {
            console.log("Erro: "+erro)
        })
    }
}
</script>

<style>
.card {
    margin: 10px;
    padding: 10px;
    border: 1px solid rgba(0, 0, 0, 0.1);
}
.title {
    font-size: 1.1em !important;
}
</style>