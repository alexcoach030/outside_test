<template>
  <div class="main">
    <div class="content" v-if="!showContent">
      <button class="button__start" @click="switchPopup()">Налоговый вычет</button>
    </div>
    <div class="default" v-if="showContent">
      <div class="default__container">
        <div class="default__info">
          <button class="button__closing" @click="switchPopup()"><img src="../assets/closing.svg" alt="close_popup"></button>
          <h1 class="default__name">Налоговый вычет</h1>
          <p class="default__description">Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер налогового вычета составляет не более 13% от своего официального годового дохода.</p>
        </div>
        <div class="default__salary">
          <h2 class="default__text">Ваша зарплата в месяц</h2>
          <form @submit.prevent="">
            <input class="default__textinput" type="number" min="0" placeholder="Введите данные" v-model="salary">
            <button class="default__submit" @click="calculate()">Рассчитать</button>
          </form>
        </div>
        <div v-if="calculated" class="calculated">
          <h2 class="default__text">Итого можете внести в качестве досрочных:</h2>
          <form>
            <div class="calculated__form" v-for="item in deduction" :key="item">
              <label class="calculated__input">
                <input type="checkbox" v-bind:id="item.year"><span>{{ item.sum }} рублей&nbsp; <span class="calculated__span"> {{ preposition(item) }} {{ item.year }}{{ ending(item) }} год</span></span>
              </label>
            </div>
          </form>
        </div>
        <div class="default__reduce">
          <h3 class="reduce__text">Что уменьшаем?</h3>
          <button @click="event => switchReduce(event)" value="payment" class="reduce__button" v-bind:class="{ 'reduce__button_active':this.paymentReduce }">Платеж</button>
          <button @click="event => switchReduce(event)" value="term" class="reduce__button" v-bind:class="{ 'reduce__button_active': !this.paymentReduce }">Срок</button>
        </div>
        <div>
          <button class="default__addbutton">Добавить</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PopupComponent",
  data(){
    return{
      message: 'popup',
      showContent: false,
      salary: '',
      paymentReduce: true,
      calculated: '',
      deduction: [],
      finalPayment: 0,
      steps: 0,
    }
  },
  methods:{
    switchPopup(){
      this.paymentReduce = false;
      return this.showContent = !this.showContent;
    },
    switchReduce(event){
      let value = event.target.value;
      if (value === 'payment'){
        return this.paymentReduce = true;
      }else return this.paymentReduce = false;
    },
    calculate(){
      if (this.salary !== ''){
        this.deduction = [];
        let salary = this.salary;
        let deduction = Math.round(salary*12*0.13);
        let steps = Math.floor(260000/deduction);
        for (let i=1; i<=steps;i++){
          let obj = {};
          obj.sum = deduction;
          obj.year = i;
          this.deduction.push(obj);
        }
        let finalPayment = Math.round(260000-deduction*steps);
        let lastObj = {};
        lastObj.sum = finalPayment;
        lastObj.year = steps+1;
        this.calculated = true;
        this.deduction.push(lastObj);
      }
    },
    ending(item){
      if(item.year === 1||item.year === 4||item.year === 5||item.year === 9 || item.year > 9){
        return '-ый';
      }else if(item.year === 2||item.year === 6||item.year === 7||item.year === 8){
        return '-ой';
      }else if(item.year === 3) return '-ий'
    },
    preposition(item){
      if (item.year === 2){
        return 'во';
      }else return 'в';
    }
  }
}
</script>

<style scoped lang="less">
  .main{
    width: 100%;
  }
  .checkbox{
    position: absolute;
    z-index: -1;
    opacity: 0;
  }
  .content{
    min-height: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
  }
  .default{
    .content;
    background: #E5E5E5;
    width: 100%;
    flex-direction: column;
  }
  .button__closing{
    .button;
    position: absolute;
    left: 94%;
    top: -3%;
    color: #EA0029;
    background-color: transparent;
    border: none;
    &:hover{
      img{
        transform: scale(1.5);
        transition-duration: 0.5s;
      }
    }
  }
  .default__container{
    width: 39%;
    background-color: #FFFFFF;
    padding: 32px;
    text-align: left;
    border-radius: 30px;
    margin-top: 120px;
    margin-bottom: 88px;
  }
  .default__text{
    font-weight: 700;
    font-size: 14px;
    line-height: 24px;
    color: #000000;
    margin: 0;
  }
  .default__info{
    position: relative;
    margin-bottom: 24px;
  }
  .default__name{
    margin: 0 0 16px 0;
    font-weight: 700;
    font-size: 28px;
    line-height: 40px;
    color: #000000;
  }
  .default__description{
    font-size: 14px;
    line-height: 24px;
    color: #808080;
    margin-bottom: 0;
  }
  .default__reduce{
    margin-top: 24px;
    display: flex;
  }
  .default__textinput{
    margin: 8px 0;
    padding: 8px 10px;
    width: 95%;
    line-height: 24px;
    font-size: 14px;
    border: 1px solid #DFE3E6;
    color: #000000;
    outline: none;
    &:hover{
      border: 1px solid #000000;
    }
    &:focus{
      border: 1px solid #DFE3E6;
    }
  }
  .default__submit{
    .default__text;
    color: #EA0029!important;
    border: none;
    background-color: transparent;
    cursor: pointer;
    padding: 0;
  }
  .button{
    outline: none;
    cursor: pointer;
  }
  .button__classic{
    .button;
    background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
    box-shadow: 0 0 24px rgba(234, 0, 41, 0.33);
    border-radius: 6px;
    color: #FFFFFF;
    font-weight: 500;
    border: none;
    &:hover{
      background: #EA0029;
    }
    &:active{
      background: #EA0029;
    }
  }
  .button__classic_big{
    .button__classic;
    width: 280px;
    height: 56px;
    font-size: 16px;
    line-height: 24px;
  }
  .button__classic_small{
    .button__classic;
    height: 40px;
    width: 288px;
    font-size: 12px;
    line-height: 16px;
  }
  .button__start{
    .button__classic_big;
    border: 1px outset white;
    &:active{
      border-style: solid;
    }
  }
  .reduce__text{
    .default__text;
    margin-top: 10px;
    margin-right: 32px;
  }
  .reduce__button{
    .button;
    .default__text;
    border-radius: 50px;
    background: #EEF0F2;
    border: none;
    padding: 7px 12px;
    margin: 0 16px 32px 0;
  }
  .reduce__button_active{
    background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
    color: #FFFFFF;
  }
  .default__addbutton{
    .button__classic_small;
    width: 100%;
  }
  .calculated{
    margin-top: 16px;
  }
  .calculated__form{
    width: 100%;
    border-bottom: 1px solid #DFE3E6;
    font-size: 14px;
    line-height: 24px;
    font-weight: 700;
    padding: 16px 0;
    color: #000000;
  }
  .calculated__span{
    font-weight: 400;
  }
  .calculated__input>input{
    .checkbox;
  }
  .calculated__input>span {
    display: inline-flex;
    align-items: center;
    user-select: none;
  }
  .calculated__input>span::before{
    content: '';
    display: inline-block;
    width: 20px;
    height: 20px;
    flex-shrink: 0;
    flex-grow: 0;
    border: 1px solid #DFE3E6;
    border-radius: 6px;
    margin-right: 0.5em;
    background-repeat: no-repeat;
    background-position: center center;
    background-size: 70% 70%;
  }
  .calculated__input>input:not(:disabled):not(:checked)+span:hover::before {
    border-color: #000000;
  }
  .calculated__input>input:not(:disabled):active+span::before {
    border-color: #FF5E56;
  }
  .calculated__input>input:checked+span::before {
    background-color: #FF5E56;
    background-image: url("../assets/checked.svg");
  }
  .calculated__input>input:disabled+span::before {
    background-color: #BEC5CC;
  }
  
  @media (max-width: 788px) {
    .default__container{
      padding: 32px 16px;
    }
    .default__name{
      font-size: 18px;
      line-height: 24px;
    }
    .default__description{
      font-size: 12px;
      line-height: 16px;
    }
    .default__reduce{
      display: block;
    }
    .reduce__text{
      margin-bottom: 30px;
    }
  }
  @media (max-width: 450px) {
    .default__name{
      font-size: 12px;
      line-height: 12px;
    }
    .default__description{
      font-size: 8px;
      line-height: 12px;
    }
    .default__text{
      font-size: 9px;
      line-height: 9px;
    }
    .default__textinput{
      width: 85%;
      line-height: 10px;
      font-size: 8px;
    }
    .default__submit{
      font-size: 11px;
    }
    .reduce__text{
      font-size: 11px;
    }
    .reduce__button{
      font-size: 11px;
      line-height: 15px;
      margin: 0 3px 26px 0;
    }
  }
</style>