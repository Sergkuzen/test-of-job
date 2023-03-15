<!--div class="mb-4">
  <h7 class="text-secondary">User 1</h7>
  <div class="col-md-8 mt-1">
    <div class="border rounded bg-light">
      <div class="d-flex justify-content-between align-items-center">
        <div class="w-75 text-break ps-3 pt-2 pb-2">
          Сообщение
        </div>
        <div class="w-25 text-end mt-auto pe-2 pt-4">
          <span class="text-black-50" style="font-size: 12px;">00.55</span>
        </div>
        <a href="javascript:void(0);" class="text-secondary" style="text-decoration: none; position: relative; bottom: 18px; left: 15px;">X</a>
      </div>
    </div>
  </div>
</div-->
<template>
  <div id="app">
    <div class="container mt-5">
      <div class="row justify-content-evenly align-items-baseline">
        <div class="col-md-6 mb-2 p-0 border rounded-3 align-self-start">
          <div class="flex-column p-0">
            <div class="h-auto m-0 bg-light p-3 rounded-top">
              <!--Header | Not coded-->
            </div>
            <div class="border-top border-bottom chat-height" id="chat_block">
              <div class="d-flex flex-column p-4">
                <!--TodoList 
                  v-bind:data_storage="resultat"
                /-->
                <div class="mb-4" v-for="(resultats, index) of resultat" :key="resultats">
                  <div class="text-secondary">{{ resultats.login }}</div>
                  <div class="col-md-8 mt-1">
                      <div class="border rounded bg-light">
                      <div class="d-flex justify-content-between align-items-center">
                          <div class="w-75 text-break ps-3 pt-2 pb-2">
                            {{ resultats.message }}
                          </div>
                          <div class="w-25 text-end mt-auto pe-2 pt-4">
                          <span class="text-black-50" style="font-size: 12px;">{{ resultats.time }}</span>
                          </div>
                          <a href="javascript:void(0);" class="text-secondary" style="text-decoration: none; position: relative; bottom: 18px; left: 15px;" @click="removeMessage(index)">&times;</a>
                      </div>
                      </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="h-auto m-0 bg-light p-5 rounded-bottom">
              <div class="text-field">
                <div class="text-field__icon">
                  <input class="text-field__input" placeholder="Сообщение" v-model="data_message">
                  <span class="text-field__aicon">
                    <img src="./assets/paper-plane-top.png" style="width: 22px;" @click="addMessage">
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-4 mt-3 border rounded-3 mb-5">
          <div class="p-4 pb-5">
            <label for="formControlInput" class="form-label text-secondary">Введите логин</label>
            <input type="text" class="form-control form-control-lg fs-6" id="formControlInput" placeholder="User-1" v-model="data_login">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//import TodoList from '@/components/TodoList.vue'
export default{
  name: 'app',
  data(){
    return {
      resultat: [],
      data_login: 'User-1',
      data_message: 'Сообщение',
      loading: false,
      count_inf: 0
    }
  },
  mounted() {
    if(localStorage.getItem('resultat')) {
      try {
        this.resultat = JSON.parse(localStorage.getItem('resultat'));
      } catch(e) {
        localStorage.removeItem('resultat');
      }
    }
    const listElm = document.querySelector('#chat_block');
    listElm.addEventListener('scroll', () => {
      if(listElm.scrollTop + listElm.clientHeight >= listElm.scrollHeight){
        this.loadMore();
      }
    });
      this.loadMore();
  },
  created() {
    setInterval(this.getNow, 1000);
  },
  methods: {
    addMessage() {
      if(!this.resultat) return;
      if(!this.data_login.trim()) return;
      if(!this.data_message.trim()) return;
        this.resultat.push({login: this.data_login, message: this.data_message, time: this.getNow()});
        //this.data_login = '';
       // this.data_message = '';
        this.saveMessage();
    },
    saveMessage() {
      let parsed = JSON.stringify(this.resultat);
      localStorage.setItem('resultat', parsed);
    },
    removeMessage(x) {
      this.resultat.splice(x,1);
      this.saveMessage();
    },
    getNow: function() {
        const today = new Date();
        const time = today.getHours() + ":" + today.getMinutes();
        return time;
    },
    loadMore(){
      if(this.resultat.length - 1 > 20){
        this.loading = true;
        setTimeout(() => {
          for(var i = 1; i <= 20; i++){
            this.resultat.push({login: 'InfinityScrollUser-' + i, message: 'Сообщение - ' + i, time: this.getNow()});
            this.saveMessage();
          }
          this.loading = false;
        }, 200);
      }
    }
  }
}
</script>
