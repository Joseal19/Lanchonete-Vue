<template>
  <div id="burger-table">
    <div>
      <Message :msg="msg" v-show="msg"/>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Molhos:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{burger.nome}}</div>
        <div>{{burger.pao}}</div>
        <div>{{burger.carne}}</div>
        <div>
          <ul>
            <li v-for="(opcional, index) in burger.opcionais " :key="index">
              {{ opcional }} 
            </li>
          </ul>
        </div>
        <div>
          <ul>
            <li v-for="(molho, index) in burger.molhos" :key="index">
              {{ molho }} 
            </li>
          </ul>
        </div>
        <div>
          <select name="status" class="status" @change="updateBurger($event, burger.id)">
            <option value="">Selecione</option>
            <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">{{ s.tipo }}</option>
          </select>
          <button id="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Message from './Message.vue';

  export default {
    name: 'Dashboard',
    data(){
      return{
        burgers: null,
        burgers_id: null,
        status: [] ,
        msg: null,
      }
    },
    components: {
      Message
    },
    methods:{
      async getPedidos(){
        const req = await fetch('http://localhost:3000/burgers');
        const data = await req.json();

        this.burgers = data;
        
        //resgatar os status
        this.getStatus();

      },
      async getStatus(){
        const req = await fetch('http://localhost:3000/status');
        const data = await req.json();

        this.status = data;

      },
      async deleteBurger(id){
        const req = await fetch(`http://localhost:3000/burgers/${id}`, {
          method: 'DELETE',
        });

        const res = await req.json();

        // colocar menssagem de sistema
        this.msg = `Pedido removido com sucesso!`;
        
        // limpar menssagem
        setTimeout(() => {
          this.msg = "";
        }, 3000);

        
        this.getPedidos();
      },
      async updateBurger(event, id){
        const option = event.target.value;
        const dataJson = JSON.stringify({status: option});
        const req = await fetch(`http://localhost:3000/burgers/${id}`, {
          method: 'PATCH',
          headers: {
            'Content-Type': 'application/json'
          },
          body: dataJson
        });

        const res = await req.json();
        
      // colocar menssagem de sistema
      this.msg = `Pedido Nº ${res.id} feito atualizado para ${res.status}!`;
      
      // limpar menssagem
      setTimeout(() => {
        this.msg = "";
      }, 3000);
      }
    },
    mounted(){
      this.getPedidos();
    }
  }
</script>

<style scoped>

  #burger-table{
    width: auto;
    margin: 0 auto;
  }

  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row{
    display: flex;
    flex-wrap: wrap;
  }

  #burger-table-heading{
    border-bottom: 3px solid #333;
    font-weight: bold;
    padding: 12px;
  }

  #burger-table-heading div,
  .burger-table-row div{
    width: 15%;
  }

  .burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #333;
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number{
    width: 5%;
  }

  select{
    padding: 12px 6px;
    margin-right: 12px;
  }

  #delete-btn{
    background-color: #fcba03;
    color: #fff;
    border: 2px solid #222;
    font-weight: bold;
    font-size: 16px;
    padding: 10px;
    margin: 0 auto;
    transition: .5s;
    cursor: pointer;
  }

  #delete-btn:hover{
    background-color: transparent;
    color: #fcba03;
  }

</style>