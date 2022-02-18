<template>
    <div id="pokemon"> 
        <div class="card">
            <div class="card-image">
                <figure>
                <img :src="currentImg" alt="Placeholder image">
                </figure>
            </div>
            <div class="card-content">
                <div class="media">
                <div class="media-content">
                    <p class="title is-4">{{num}} - {{name | upper}} </p>
                    <p class="subtitle is-6">{{pokemon.type}}</p>
                </div>
                </div>

                <div class="content">
                    <button @click="changeSprite" class="button is-medium is-fullwidth"> Mudar Sprite </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios';
export default{
    created: async function() {
        try {
            const { data } = await axios.get(this.url)
            this.pokemon.type = data.types[0].type.name
            this.pokemon.front = data.sprites.front_default
            this.pokemon.back = data.sprites.back_default
            this.currentImg = this.pokemon.front; 
        }catch(err){
            console.log(err)
        }
        
    },
    data() {
        return {
            isFront: true,
            currentImg: '',
            pokemon: {
                type: [],
                front: '',
                back: ''
            }
        }
    },
    props: {
        num: Number,
        name: String,
        url: String
    },
    filters: {
        upper: function(value) {
            let newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }
    },
    methods: {
        changeSprite: function() {
            if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.back;
            }else{
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        }
    }
}

</script>

<style>
#pokemon{
    margin: 1rem 0;
}
</style>
