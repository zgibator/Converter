<template>
  <div>
    <!--<img alt="Vue logo" src="./assets/logo.png">-->
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <h1>Converter</h1>
    <br />

    <table v-for="elem in array" v-bind:key="elem">
      <td>{{ elem.ccy }}</td>
      <td><button @click="q(elem.ccy)">Выбрать</button></td>
    </table>
    <br />
    <a>Конвертировать: {{ arr.ccy }}, В: UAH</a>
    <br />

    <br />
    <a>Введите сумму</a>
    <input type="text" v-model="ent" />
    <br />
    <span>{{ roundEnt }}</span>
    <br />
    <button @click="calc">Вычислить</button>
    <br />
    <a>Покупка: {{ BUY }}</a>
    <br />
    <a>Продажа: {{ SALE }}</a>
  </div>
</template>

<script>
//import Poshta from './components/HelloWorld.vue'
//import Vue from 'vue'
import axios from "axios";
//import VueAxios from 'vue-axios'

export default {
  name: "App",
  data() {
    return {
      array: [],
      arr: [],
      kName: "",
      ent: "",
      BUY: "",
      SALE: "",
    };
  },
  mounted: function () {
    axios
      .get("https://api.privatbank.ua/p24api/pubinfo?json&exchange&coursid=5")
      .then((response) => {
        console.log(response.data);
        this.array = response.data;
      });
  },
  methods: {
    q: function (kName) {
      this.arr = this.array.find((element) => {
        return element.ccy == kName;
      });
    },
    calc: function () {
      this.BUY = this.ent * this.arr.buy;
      this.SALE = this.ent * this.arr.sale;
    },
  },
  computed: {
    roundEnt: function () {
      return Math.round(this.ent * 100) / 100;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
