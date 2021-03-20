<template>
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
            urlatual: ''
        }
    },
    methods: {
        fetchData(){
            let req = new Request(this.urlApi);
            fetch(req)
                .then((resp)=>{
                    if(resp.status === 200) return resp.json();
                })
                .then((data)=>{
                    this.urlprox = data.next;
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
        }
    },
    prox()
    {
        this.urlatual = this.urlprox;
        this.fetchData();
    },
    created() {
        this.urlatual = this.urlApi;
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
</style>
