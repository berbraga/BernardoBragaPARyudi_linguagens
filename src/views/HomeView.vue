<template>
  <div class="container leftText">
    <div class="div">
      <p class="zero leftText">Digite a sequencia aqui:</p>
      <textarea id="input" placeholder=" " class="Input"></textarea>
    </div>
    <div class="boxButton">
      <button @click="this.clear()">Limpar</button>
      <button @click="this.main()">Analisar</button>
    </div>
    <div class="div">
      <p class="zero leftText">Resultado sairá aqui</p>
      <textarea name="results" id="results" placeholder=" "></textarea>
    </div>
  </div>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue";
import { routeLocationKey } from "vue-router";

export default {
  name: "HomeView",
  components: {
    HelloWorld,
  },
  data() {
    return {
      alfabeto: ["a", "b", "c", "d"],
      VALID: "sentença válida:",
      aritimetic: "operador aritmético",
      sentencaInvalida: "ERRO => sentença inválida",
      simbuloInvalido: "ERRO => símbolo(s) inválido(s)",
      aritimeticOperators: ["+", "-", "/", "*"],
      separadores: ["+", "-", "/", "*", "$", " ", "\n", "&#92;"],

      tabelaT: [
        /* | A   B   C   D    FI  */
        /* 0 */ [12, 12, 12, 3, 12],
        /* 1 */ [12, 8, 12, 3, 12],
        /* 2 */ [5, 6, 12, 0, 12],
        /* 3 */ [12, 12, 12, 0, 12],
        /* 4 */ [12, 6, 12, 0, 12],
        /* 5 */ [2, 11, 12, 12, 12],
        /* 6 */ [12, 12, 7, 12, 12],
        /* 7 */ [12, 9, 12, 12, 12],
        /* 8 */ [12, 12, 10, 12, 12],
        /* 9 */ [12, 12, 4, 12, 12],
        /* 10 */ [12, 11, 12, 12, 12],
        /* 11 */ [12, 12, 1, 12, 12],
      ],
      EF: [1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    };
  },
  methods: {
    indiceSimbolo: function (simbolo) {
      if (simbolo == "a") return 0;
      else if (simbolo == "b") return 1;
      else if (simbolo == "c") return 2;
      else if (simbolo == "d") return 3;
      else return 4;
    },
    getSentencas: function (token) {
      let sentencas = [];
      let sentenca = "";

      for (const element of token) {
        if (this.separadores.includes(element)) {
          sentenca += element;
          sentencas.push(sentenca);
          sentenca = "";
        } else {
          
          sentenca += element;
        }
      }
      return sentencas;
    },
    inAlfhabeth: function (sentenca) {
      sentenca = sentenca.trim();
      for (let i = 0; i <= sentenca.length - 1; i++) {
        if (sentenca[i] !== "$") {
          if (!this.alfabeto.includes(sentenca[i])) {
            return false;
          }
        }
      }
      return true;
    },
    generic: function (sentencas) {
      const results = document.getElementById("results");
      let estado = 2;
      
      for (const element of sentencas) {
        if (!this.inAlfhabeth(element)) {
          let aritmethicSimbol = false;
          let invalidSimbol = false;

          for (let i = 0; i < element.length; i++) {
            if (this.aritimeticOperators.includes(element[i])) {
              aritmethicSimbol = true;
            } else if (element[i] !== " ") {
              invalidSimbol = true;
              break;
            }
          }

          if (aritmethicSimbol && !invalidSimbol) {
            results.value += `${this.aritimetic}                      ${element}\n`;
          } else if (invalidSimbol) {
            results.value += `${this.simbuloInvalido}           ${element}\n`;
          } else {
            results.value += `${this.sentencaInvalida}                ${element}\n`;
          }
        } else {
          for (let i = 0; i <= element.length - 1; i++) {
            let indice = this.indiceSimbolo(element[i]);

            if (estado === 12) {
              break;
            }
            estado = this.tabelaT[estado][indice]; //erro aqui
          }
          if (this.EF[estado] === 1) {
            results.value += `${this.VALID}                         ${element}\n`;
          } else {
            if (element !== " ") {
              results.value += `${this.sentencaInvalida}                ${element}\n`;
            }
          }

          estado = 2;
        }
      }
    },
    teste: function (array) {
      
      const regex = /([-+*/])$/;

      const novoArray = array.map(function (element) {
          let match = element.match(regex);
          if (match) {
            return [element.slice(0, -1), match[1]];
          } else {
            return [element];
          }
        })
        .flat()
        .filter(function (element) {
          // Remove elementos vazios ou com espaços
          return element.trim() !== "";
        });

      return novoArray;
    },
    main: function () {
      console.clear();
      document.getElementById("results").value = "";
      const input = document.getElementById("input");
      // const results = document.getElementById('results');
      let toke = input.value;
      const token = toke + "$";
      const sentencas = this.getSentencas(token);
    
      const teste = this.teste(sentencas);
      
      let a = [];
      for (const element of teste) {
        a.push(element.replace(/\n/g, "").trim());
      }
      a[a.length - 1] = a[a.length - 1].replace("$", "").trim();
      this.generic(a);
      // this.generic(arraySemVazios);
    },
    clear: function () {
      document.getElementById("input").value = "";
      document.getElementById("results").value = "";
    },
  },
};
</script>

<style scoped>
.zero {
  padding: 0px !important;
  margin: 0px !important;
}

.leftText {
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

.div {
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
