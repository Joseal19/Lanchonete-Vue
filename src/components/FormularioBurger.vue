<template>
  <div>
    <Message :msg="msg" v-show="msg"/>
    <div>
      <form  id="burger-form" @submit="createBurger">
        <div class="input-container">
          <label for="nome">Nome do Cliente:</label>
          <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite seu nome">
        </div>
        <div class="input-container">
          <label for="pao">Escolha o pão:</label>
          <select name="pao" id="pao" v-model="pao">
            <option value="">Selecione o seu pão</option>
            <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
          </select>
        </div>
        <div class="input-container">
          <label for="carne">Escolha sua carne:</label>
          <select name="carne" id="carne" v-model="carne">
            <option value="">Selecione o tipo de carne</option>
            <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
          </select>
        </div>
        <div id="opcionais-container" class="input-container">
          <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
          <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.key">
            <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
            <span>{{ opcional.tipo }}</span>
          </div>
        </div>
        <div id="molhos-container" class="input-container">
          <label id="molhos-title" for="molhos">Selecione seus molhos:</label>
          <div class="checkbox-container" v-for="molho in molhosdata" :key="molho.key">
            <input type="checkbox" name="molhos" v-model="molhos" :value="molho.tipo">
            <span>{{ molho.tipo }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Criar meu Burger!">
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from './Message.vue';

export default {
  name: 'FormularioBurger',

  data(){
    return{
      paes: null,
      carnes: null,
      opcionaisdata: null,
      molhosdata: null,
      name:null,
      pao: null,
      carne: null,
      opcionais: [],
      molhos: [],
      msg: null,

    }
  },
  methods:{
    async getIngresientes(){
      const req = await fetch('http://localhost:3000/ingredientes');
      const data = await req.json();
      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
      this.molhosdata = data.molhos;
    },
    async createBurger(e){
      e.preventDefault();
      
      const data = {
        nome: this.nome,
        pao: this.pao,
        carne: this.carne,
        opcionais: Array.from(this.opcionais),
        molhos: Array.from(this.molhos),
        status: "Solicitado",
      }
      // madar em forma de  texto para o "servidor" 
      const dataJson = JSON.stringify(data);

      const req = await fetch('http://localhost:3000/burgers', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: dataJson
      });

      const res = await req.json();
      
      // colocar menssagem de sistema
      this.msg = `Pedido Nº ${res.id} feito com sucesso!`;
      
      // limpar menssagem
      setTimeout(() => {
        this.msg = "";
      }, 3000);

      // limpar formulario
      this.nome = "";
      this.pao = "";
      this.carne = "";
      this.opcionais = "";
      this.molhos = "";

    }
  },
    mounted(){
      this.getIngresientes();
    },
    components:{
      Message
    }
}
</script>


<style scoped>
  #burger-form{
    max-width: 300px;
    margin: 0 auto;
  }

  .input-container{
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  label{
    font-weight: bold;
    margin-bottom: 15px;
    color : #222;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
  }

  input, select{
    padding: 5px 10px;
    width: 300px;
  }

  #opcionais-container,
  #molhos-container{
    flex-direction: row;
    flex-wrap: wrap;
  }

  #opcionais-title,
  #molhos-title{
    width: 100%;
  }

  .checkbox-container{
    display: flex;
    align-items: flex-start;
    margin-bottom: 20px;
    width: 50%;
  }

  .checkbox-container span,
  .checkbox-container input{
    width: auto;
  }

  .checkbox-container span{
    margin-left: 6px;
    font-weight: bold;
  }

  .submit-btn{
    background-color: #fcba03;
    color: #fff;
    border: 2px solid #222;
    padding: 10px;
    font-weight: bold;
    cursor: pointer;
    transition: .5s;
    margin: 0 auto;
  }

  .submit-btn:hover{
    background-color: transparent;
    color: #fcba03;
  }

</style>