<template>
  <div class="container leftText">
    <div class="div">

      <p class="zero leftText">digite a sequencia aqui:</p>
      <textarea id="input" placeholder=" " class="Input"></textarea>
    </div>
    <div class="boxButton">
      <button @click=" this.clear()">Limpar</button>
      <button @click=" this.main()">Analizar</button>
    </div>
    <div class="div">
      <p class="zero leftText">Resultado sairá aqui</p>
      <textarea name="results" id="results" placeholder=" "></textarea>
    </div>
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'
import {routeLocationKey} from 'vue-router'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  },
  data(){
    return{
      alfabeto: ['a','b','c','d'],
      VALID: 'sentença válida:',
      aritimetic:  'operador aritmético',
      sentencaInvalida: 'ERRO => sentença inválida',
      simbuloInvalido: 'ERRO => símbolo(s) inválido(s)',
      aritimeticOperators: ['+','-','/','*'],
      separadores: ['+','-','/','*','$',' '],


      tabelaT:[
      /* | A   B   C   D    FI  */  
/* 0 */ [ 12, 12, 12,  3 , 12 ],
/* 1 */ [ 12,  8, 12,  3 , 12 ],
/* 2 */ [  5,  6, 12,  0 , 12 ],
/* 3 */ [ 12, 12, 12,  0 , 12 ],
/* 4 */ [ 12,  6, 12,  0 , 12 ],
/* 5 */ [  2, 11, 12, 12 , 12 ],
/* 6 */ [ 12, 12,  7, 12 , 12 ],
/* 7 */ [ 12,  9, 12, 12 , 12 ],
/* 8 */ [ 12, 12, 10, 12 , 12 ],
/* 9 */ [ 12, 12,  4, 12 , 12 ],
/* 10 */[ 12, 11, 12, 12 , 12 ],
/* 11 */[ 12, 12,  1, 12 , 12 ],
      ],
      EF: [1,1,0,0,0,0,0,0,0,0,0,0,0],
    };
  },
  methods:{
    indiceSimbolo: function (simbolo) {
      if(simbolo == 'a') return 0
      else if(simbolo == 'b') return 1
      else if(simbolo == 'c') return 2
      else if(simbolo == 'd') return 3
      else return 4
    },
    getSentencas: function (token){
      console.log(token)
      let sentencas = [];
      let sentenca = '';
      
      for (const element of token) {
        if(this.separadores.includes(element)){
            sentenca +=  element;
            sentencas.push(sentenca);
            sentenca = '';  
        }else{
          sentenca +=  element
        }
      }
      return sentencas;
    },
    inAlfhabeth: function(sentenca){
      sentenca = sentenca.trim();
      for(let i = 0; i <= sentenca.length - 1 ; i++  ){
        if(sentenca[i]!=='$'){
          if (!this.alfabeto.includes(sentenca[i])) {
            return false;
          }
        }
      }
      return true;
    },
    generic: function(sentencas) {
      const results = document.getElementById('results');
      let estado = 2;

      for (const element of sentencas) {
        if (!this.inAlfhabeth(element)) {
          let aritmethicSimbol = false;
          let invalidSimbol = false;

          console.log('ta no else');
          console.log(element);

          for (let i = 0; i < element.length; i++) {
            console.log('bernardo');
            console.log(this.aritimeticOperators.includes(element[i]));

            if (this.aritimeticOperators.includes(element[i])) {
              console.log('simbulo: ', element[i]);
              aritmethicSimbol = true;
            } else if (element[i] !== ' ') {
              invalidSimbol = true;
              break; 
            }
          }
          console.log(aritmethicSimbol, invalidSimbol);

          if (aritmethicSimbol && !invalidSimbol) {
            results.value += `${this.aritimetic} ${element}\n`;
          } else if (invalidSimbol) {
            results.value += `${this.simbuloInvalido} ${element}\n`;
          } else {
            results.value += `${this.sentencaInvalida} ${element}\n`;
          }
        } 
        else {
          console.log('ta no if');
          console.log(element);

          for (let i = 0; i < element.length-1; i++) {
            let indice = this.indiceSimbolo(element[i]);
            console.log('estado: ', estado ,indice);
            if (estado === 12) {
              break;
            }
            estado = this.tabelaT[estado][indice]; //erro aqui
          }
          console.log('aaaaaaaaaaaaaaaaaaaa',estado);
          if (this.EF[estado] === 1) {
            
            results.value +=  `${this.VALID} ${element}\n`;
          } else {
            results.value += `${this.sentencaInvalida} ${element}\n`;
          }

          estado = 2;
        }
      }
    },

    main:function(){
    
      console.clear();
      const input = document.getElementById('input');
      // const results = document.getElementById('results');
      let toke = input.value;
      const token = toke+"$" ;
      const sentencas = this.getSentencas(token);
      console.log(sentencas)
      this.generic(sentencas);

    },
    clear:function(){
      document.getElementById('input').value = '';
      document.getElementById('results').value = '';
    },
  },

}
</script>

<style scoped>
.zero{
  padding: 0px !important;
  margin: 0px !important;
}
.leftText{
  text-align: left;
}
.container {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  align-content: center;
  align-items: center;
  justify-content: center;
}
.div{
  width: 100%;
}
#results {
  width: 100%;
  min-height: 200px;
  background-color: rgb(255, 255, 255);
  
}

.Input {
  width: 100%;
  min-height: 200px;
  background-color: rgb(255, 255, 255);
  
}
</style>