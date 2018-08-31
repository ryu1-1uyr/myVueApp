<!--<template>-->
  <!--<div id="app">-->
    <!--<img src="./assets/logo.png">-->
    <!--<h1>{{ msg }}</h1>-->
    <!--<h2>Essential Links</h2>-->
    <!--<ul>-->
      <!--<li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>-->
      <!--<li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>-->
      <!--<li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>-->
      <!--<li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>-->
    <!--</ul>-->
    <!--<h2>Ecosystem</h2>-->
    <!--<ul>-->
      <!--<li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>-->
      <!--<li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>-->
      <!--<li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>-->
      <!--<li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>-->
    <!--</ul>-->
  <!--</div>-->
<!--</template>-->

<!--<script>-->
<!--export default {-->
  <!--name: 'app',-->
  <!--data () {-->
    <!--return {-->
      <!--msg: 'Welcome to Your Vue.js App'-->
    <!--}-->
  <!--}-->
<!--}-->
<!--</script>-->

<template>
  <div>
    <myheader></myheader>
    <p v-if="msg.length > 0">
      {{msg}}
    </p>
    <p v-else>
      no text
    </p>
    <input type="text" v-model="msg">
    <button @click="search()">search</button>
    <button @click="getRandomPokemon()">random</button>
    <p>{{keepmsg}}</p>

    <div>
      <p>体力:{{pokeH}}, 攻撃:{{pokeA}}, 防御:{{pokeB}}</p>
      <p>特攻:{{pokeC}}, 特防:{{pokeD}}, 素早さ:{{pokeS}}</p>
      <img :src="`${this.img}`" >
      <img :src="`${this.img_b}`">
      <graph-sample
        :data1="data1"
        :data2="data2"
      />

    </div>

  </div>
</template>

<script>
  import myheader from './components/myheader'
  import Pokemon from 'pokemon'
  import { Radar } from 'vue-chartjs'

  const url = "https://pokeapi.co/api/v2/pokemon/";

  export default {
    extends: Radar,
    mounted () {
      // Overwriting base render method with actual data.
      this.renderChart({
        labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
        datasets: [
          {
            label: 'GitHub Commits',
            backgroundColor: '#f87979',
            data: [40, 20, 12, 39, 10, 40, 39, 80, 40, 20, 12, 11]
          }
        ]
      })
    },

    components: {
      myheader ,
      GraphSample: Radar

    },
    data () {
      return {
        msg: Pokemon.random('ja'),
        keepmsg: '',
        pokeid:'',
        pokeS:'',
        pokeD:'',
        pokeC:'',
        pokeB:'',
        pokeA:'',
        pokeH:'',
        img:'',
        img_b:'',
        function() {
          return {
            data1: 40,
            data2: 80,
          }
        }
      }
    },
    methods: {
      search () { // フェッチなどを実装するとこ
        let pokeId = Pokemon.getId(this.msg,'ja');
        fetch("https://pokeapi.co/api/v2/pokemon/"+pokeId)
          .then((response) => response.json())
          .then((json) => {
            this.pokeS = json["stats"][0]["base_stat"];
            this.pokeD = json["stats"][1]["base_stat"];
            this.pokeC = json["stats"][2]["base_stat"];
            this.pokeB = json["stats"][3]["base_stat"];
            this.pokeA = json["stats"][4]["base_stat"];
            this.pokeH = json["stats"][5]["base_stat"];

            fetch(json["forms"][0]["url"])
              .then((response2)=> response2.json())
              .then((json2) => {
                this.img = json2["sprites"]["front_default"];
                this.img_b = json2["sprites"]["back_default"];
              })
            })

      },
      keep () {
        this.keepmsg = this.msg
      },
      put () {
        this.msg = this.keepmsg
      },
      getRandomPokemon (){
        this.msg = Pokemon.random('ja');
      }
    }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
