<template>
    <div class="procura">
        <form @submit.prevent="definirUrlPokemon">
            <input type="text" v-model="dadobusca">
            <p @click="definirUrlPokemon">Buscar</p>
        </form>
               
    </div>
</template>

<script>

export default {
    props: 
    {
        urlApi:String
    },
    data: ()=> {
        return {
            dadobusca: '',
        }
    },
    methods: {
        definirUrlPokemon()
        {
            if(this.dadobusca !=='')
            this.$emit('definirUrlPokemon',this.urlApi + this.dadobusca);
        }   
    }
}
</script>
<style scoped>
.procura
    {
        position: relative;
        width: 100%;
        max-width: 510px;
        padding-bottom: 20px;
    }
input
    {
        float:left;
        border: none;
        outline: none;
        border-radius: 5px;
        padding: 10px 40px 10px 10px;
        width: calc(90% - 50px);
        font-size: 1rem;
        box-shadow: 0 15px 30px rgba(0, 0, 0, .2)
                    0 10px 10px rgba(0, 0, 0, .2);
    }
p
    {
        top:10px;
        right: 10px;
        font-size: 1.85rem;
        color:#FFFFFF;
        cursor:pointer;
        background-color: #999999;
        border-radius: 5px;
    }
p:hover
    {
        background-color: #3c8cf5;
    }
</style>
