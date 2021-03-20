<template>
<div>
    <div class="row">
        <div class="col-sm-3 offset-sm-3">
            <p class="botao" 
            v-if="botaoante"
            @click="paginaAnterior">Anterior</p>
        </div>
        <div class="col-sm-3">
            <p class="botao" 
            v-if="botaoprox"
            @click="proximaPagina">Proximo</p>
        </div>
    </div>
    <div class="row">
        <div class="col-sm-6 offset-sm-3">
            <div class="pokelist list">
            <article v-for="(pokemon, index) in pokemons" 
            :key="'poke'+index"
            @click="definirUrlPokemon(pokemon.url)">
            <img :src="urlImagem + pokemon.id + '.png'" width="96" height="96" />
            <h3>{{ pokemon.name }}</h3>
            </article>
            </div>
        </div>
    </div>
</div>
    
</template>
<script>

export default {
    name: 'ListaPokemon',
    props: 
    {
        urlImagem:String,
        urlApi:String
    },
    data: ()=> {
        return {
            pokemons: [],
            urlprox: '',
            urlAnterior: '',
            urlatual: '',
            botaoprox: false,
            botaoante: false
        }
    },
    methods: {
        fetchData(){
            let req = new Request(this.urlatual);
            fetch(req)
                .then((resp)=>{
                    if(resp.status === 200) return resp.json();
                })
                .then((data)=>{
                    if(data.next !== null)
                    {
                        this.urlprox = data.next;
                        this.botaoprox = true;
                    }
                    if(data.previous !== null)
                    {
                        this.urlAnterior = data.previous;
                        this.botaoante = true;
                    }
                    data.results.forEach(pokemon => {
                        pokemon.id = pokemon.url.split('/')
                        .filter(function(part) { return !!part }).pop();
                        this.pokemons.push(pokemon);
                    });
                })
                .catch((erro) =>{
                    console.log(erro);
                })
        },
        definirUrlPokemon(url)
        {
            this.$emit('definirUrlPokemon',url);
        },
        proximaPagina()
        {
            this.urlatual = this.urlprox;
            this.pokemons = [];
            this.urlprox = null;
            this.botaoprox = false;
            this.botaoante = false;
            this.urlAnterior = null;
            let req = new Request(this.urlatual);
            fetch(req)
                .then((resp)=>{
                    if(resp.status === 200) return resp.json();
                })
                .then((data)=>{
                    if(data.next !== null)
                    {
                        this.urlprox = data.next;
                        this.botaoprox = true;
                    }
                    if(data.previous !== null)
                    {
                        this.urlAnterior = data.previous;
                        this.botaoante = true;
                    }
                    data.results.forEach(pokemon => {
                        pokemon.id = pokemon.url.split('/')
                        .filter(function(part) { return !!part }).pop();
                        this.pokemons.push(pokemon);
                    });
                })
                .catch((erro) =>{
                    console.log(erro);
                })

        },
        paginaAnterior()
        {
            this.urlatual = this.urlAnterior;
            this.pokemons = [];
            this.urlprox = null;
            this.botaoprox = false;
            this.botaoante = false;
            this.urlAnterior = null;
            let req = new Request(this.urlatual);
            fetch(req)
                .then((resp)=>{
                    if(resp.status === 200) return resp.json();
                })
                .then((data)=>{
                    if(data.next !== null)
                    {
                        this.urlprox = data.next;
                        this.botaoprox = true;
                    }
                    if(data.previous !== null)
                    {
                        this.urlAnterior = data.previous;
                        this.botaoante = true;
                    }
                    data.results.forEach(pokemon => {
                        pokemon.id = pokemon.url.split('/')
                        .filter(function(part) { return !!part }).pop();
                        this.pokemons.push(pokemon);
                    });
                })
                .catch((erro) =>{
                    console.log(erro);
                })
        }
    },
    created() {
        if(this.urlatual === '')
        {
            this.urlatual = this.urlApi;
        }
        this.fetchData();
    }
}
</script>

<style scoped>
.list
{
    display: grid;
    grid-template-columns: repeat(auto-fit,minmax(150px,1fr));
    grid-gap:10px;
    width: 100%;
    max-width: 900px;
}
article
{
    border-radius: 5px;
    height: 150px;
    background-color: #efefef;
    text-align: center;
    text-transform: capitalize;
    cursor: pointer;
    box-shadow: 0px 15px 30px rgba(0, 0, 0,.2),
                0px 10px 10px rgba(0, 0, 0,.2);
}
h3
{
    margin: 0;
}
.botao
{
    background-color: rgba(90, 224, 146, 0.933);
    color:#FFFFFF;
    border-radius: 5px;
    font-size: 1.50rem;
    cursor: pointer;
}
.botao:hover
{
    background-color: rgba(224, 90, 90, 0.933);   
}
</style>
