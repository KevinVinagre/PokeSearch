<template>
    <div class="detalhes">
        <div class="canvas-detalhe" v-if="mostrar">
            <div v-if="pokemon" class="imagem">
                <img :src="urlImagem + pokemon.id +'.png'" alt="">
            </div>
            <div v-if="pokemon" class="dado">
                <h2>{{pokemon.name}}</h2>
                <div class="propriedade">
                    <div class="esquerda">Experiencia Base:   </div>
                    <div class="direita">{{pokemon.base_experience}} XP</div>
                </div>
                <div class="propriedade">
                    <div class="esquerda">Altura:   </div>
                    <div class="direita">{{pokemon.height /10}} XP</div>
                </div>
                <div class="propriedade">
                    <div class="esquerda">Peso:   </div>
                    <div class="direita">{{pokemon.weight /10}} XP</div>
                </div>
                <h3>Tipo de Pokemon:</h3>
                <div class="tipos">
                    <div class="tipo" 
                    v-for="(tipo, index) in pokemon.types"
                    :key="'tipo'+index">
                    {{ tipo.type.name }}
                    </div>
                </div>
                <h3>Habilidades:</h3>
                <div class="habilidades">
                    <div class="habilidade" 
                    v-for="(habilidade, index) in pokemon.abilities"
                    :key="'habilidade'+index">
                    {{ habilidade.ability.name }}
                    </div>
                </div>
            </div>
            <h2 v-else>O pokemon não foi encontrado</h2>
        </div>
        <button class="fechar" @click="fecharDetalhes">X</button>
    </div>
</template>
<script>
export default {
    props: 
    {
        urlPokemon: String,
        urlImagem: String
    },
    data: ()=> {
        return {
            mostrar: false,
            pokemon: {}
        }
    },
    methods: {
        fetchData() 
        {
            let req = new Request(this.urlPokemon);
            fetch(req)
                .then((resp)=>{
                    if(resp.status === 200) return resp.json();
                })
                .then((data)=>{
                    this.pokemon = data;
                    this.mostrar = true;
                })
                .catch((erro) =>{
                    console.log(erro);
                })
        },
        fecharDetalhes()
        {
            this.$emit('fecharDetalhes');
        }
    },
    created()
    {
        this.fetchData();
    }
}
</script>
<style scoped>
.detalhes
{
    display: flex;
    justify-content: center;
    align-items: flex-start;
    position: fixed;
    top: 0;
    left: 0;
    padding: 90px 10px 10px;
    width:100%;
    height:100%;
    background: rgba(0, 0, 0, .7);
}
.canvas-detalhe
    {
        display: flex;
        justify-content: center;
        align-items: center;
        position: relative;
        width: 100%;
        max-width: 510px;
        padding: 50px 0 0;
        background-color: #FFFFFF;
        border-radius: 5px;
        box-shadow: 0px 15px 30px rgba(0, 0, 0,.2),
                    0px 10px 10px rgba(0, 0, 0,.2);
    }
.imagem
    {
        display: flex;
        justify-content: center;
        align-content: center;
        position: absolute;
        top: -60px;
        width: 120px;
        height: 120px;
        background-color: #555555;
        border-radius: 50%;
        overflow: hidden;
        box-shadow: 0px 15px 30px rgba(0, 0, 0,.2),
                    0px 10px 10px rgba(0, 0, 0,.2);

    }
h2
    {
        text-transform: capitalize;
    }
.dado
    {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column;
        width: 100%;
        margin-bottom: 40px;
        margin-top: 20px;
    }
.propriedade
    {
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid #cccccc;
        margin-bottom: 10px;
    }
.esquerda
    {
        float: left;
    }
.direita
    {
       float: right;
    }
h3
    {
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid #cccccc;
    }
.tipos,.habilidades
    {
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 90%;
        max-width: 400px;
    }
.tipo,.habilidade
    {
        margin: 0 10px 10px 0;
        padding: 5px 10px;
        border-radius: 20px;
        color:#FFFFFF;
        font-size: 1rem;
        letter-spacing: 2px;
        text-transform: capitalize;
        word-wrap: none;
        word-break: keep-all;
    }
.tipo
    {
        background-color: #7bbcfa;
    }
.habilidade
    {
        background-color: #fa8ba3;
    }
.fechar
    {
        outline: none;
        border: none;
        border-radius: 5px;
        background-color: #e67e7e;
        color: #FFFFFF;
        padding: 10px 20px;
        margin-top: 20px;
        margin-bottom: 20px;
        font-size:1.2rem;
        cursor: pointer;
    }
i
    {
        font-size: 2rem;
        color: #fefefe;
    }
</style>
