<template>
    <div class="pokemon">
        <div class="card">
        <div class="card-image">
            <figure>
            <img :src="currentImg" alt="{{ name }}">
            </figure>
        </div>
        <div class="card-content">
            <div class="media">
            <div class="media-content">
                <p class="title is-4">{{ num }} - {{ upper(name) }}</p>
                <p class="subtitle is-6">{{ pokemon.type }}</p>
            </div>
            </div>
            <div class="content">
                <button class="button is-medium is-fullwidth" @click="changeSprite">Mudar sprite</button>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
import axios from "axios"

export default {
    props: {
        num: Number,
        name: String,
        url: String
    },
    data() {
        return {
            isFront: true,
            currentImg: '',
            pokemon: {
                type: '',
                front: '',
                back: ''
            }
        }
    },
    created: function () {
        axios.get(this.url).then(res => {
            this.pokemon.type = res.data.types[0].type.name
            this.pokemon.front = res.data.sprites.front_default
            this.pokemon.back = res.data.sprites.back_default
            this.currentImg = this.pokemon.front
        })
    },
    methods: {
        upper(value) {
            return value[0].toUpperCase() + value.slice(1)
        },
        changeSprite() {
            if(this.isFront) {
                this.isFront = false
                this.currentImg = this.pokemon.back
            } else {
                this.isFront = true
                this.currentImg = this.pokemon.front 
            }
        }
    }
}
</script>

<style scoped>
.pokemon{
    margin-top: 2%
}
</style>