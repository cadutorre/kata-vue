<template>
  <b-container fluid>
    <b-row>
      <b-col cols="6">
        <b-row
          v-for="(linha,x) in mapState"
          :key="x"
        >
          <b-col>
            <div
              class="borda"
              v-for="(item,y) in linha"
              :key="y"
            >
              <div
                :class="celulaVivaOuMortaHandler(item)"
                @click="celulaClickHandler(x,y)"
              ></div>
            </div>
          </b-col>
        </b-row>
      </b-col>
      <b-col cols="6">
        <b-row align-h="center">
          <b-col>
            <h1>Jogo da Vida</h1>
          </b-col>
        </b-row>
        <b-row>
          <b-col>
            <ul>
              <li>Qualquer célula viva com menos de dois vizinhos vivos morre de solidão.</li>
              <li>Qualquer célula viva com mais de três vizinhos vivos morre de superpopulação.</li>
              <li>Qualquer célula com exatamente três vizinhos vivos se torna uma célula viva.</li>
              <li>Qualquer célula com dois vizinhos vivos continua no mesmo estado para a próxima geração.</li>
            </ul>
          </b-col>
        </b-row>
        <b-row align-h="center">
          <b-col cols="4">
            <b-button @click="novaGeracaoHandler">Nova Geração</b-button>
          </b-col>
          <b-col cols="4">
            <b-button
              :variant="geracaoAutomatica?'success':'danger'"
              @click="geracaoAutomaticaHandler"
            >Geração Atutomatica</b-button>
          </b-col>
        </b-row>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import { setTimeout } from 'timers';
export default {
  data () {
    return {
      tamanhoMatriz: 20,
      mapState: [],
      geracaoAutomatica: false
    }
  },
  beforeMount () {
    for (let x of range(0, this.tamanhoMatriz - 1)) {
      this.mapState.push([]);
      for (let y of range(0, this.tamanhoMatriz - 1)) {
        this.mapState[x].push({ atual: false, novo: false });
      }
    }
  },
  methods: {
    celulaClickHandler (x, y) {
      this.mapState[x][y].atual = !this.mapState[x][y].atual;
      this.$set(this.mapState[x], y, this.mapState[x][y]);
    },
    celulaVivaOuMortaHandler (state) {
      return state.atual ? "celula-viva" : "celula-morta";
    },
    novaGeracaoHandler () {
      this.mapState.forEach((linha, x) => {
        linha.forEach((item, y) => {
          let vizinhosVivos = 0;
          for (let vizinho of vizinhos(this.mapState, x, y)) {
            vizinho.atual && vizinhosVivos++;
          }
          if (item.atual && vizinhosVivos < 2) {
            this.mapState[x][y].novo = false;
            this.$set(this.mapState[x], y, this.mapState[x][y]);
          } else if (item.atual && vizinhosVivos > 3) {
            this.mapState[x][y].novo = false;
            this.$set(this.mapState[x], y, this.mapState[x][y]);
          } else if (vizinhosVivos == 3) {
            this.mapState[x][y].novo = true;
            this.$set(this.mapState[x], y, this.mapState[x][y]);
          }
        })
      });

      this.mapState.forEach((linha, x) => {
        linha.forEach((item, y) => {
          this.mapState[x][y].atual = this.mapState[x][y].novo;
          this.$set(this.mapState[x], y, this.mapState[x][y]);
        })
      })

    },
    async geracaoAutomaticaHandler () {
      this.geracaoAutomatica = !this.geracaoAutomatica;
      while (this.geracaoAutomatica) {
        await new Promise(resolve => setTimeout(resolve, 500));
        this.novaGeracaoHandler();
      }
    }
  }
}
function* range (start, end) {
  yield start;
  if (start === end) return;
  yield* range(start + 1, end);
}

function* vizinhos (map, x, y) {
  console.log("tamanho: ", map.length)
  console.log("vizinhos de ", x, "/", y);
  //19/0
  x > 0 && y > 0 ? yield map[x - 1][y - 1] : false;//18/-1
  x > 0 ? yield map[x - 1][y] : false;//18/0
  x > 0 && y < map.length - 1 ? yield map[x - 1][y + 1] : false;//18/1
  y < map.length - 1 ? yield map[x][y + 1] : false;//19/1
  y > 0 ? yield map[x][y - 1] : false;//19/-1
  x < map.length - 1 && y < map.length - 1 ? yield map[x + 1][y + 1] : false;//20/1
  x < map.length - 1 ? yield map[x + 1][y] : false;//20/0
  x < map.length - 1 && y > 0 ? yield map[x + 1][y - 1] : false;//20/-1
}
</script>

<style>
.borda {
  border: 1px solid #000;
  height: 30px;
  width: 30px;
  float: left;
  cursor: pointer;
}

.celula-viva {
  background-color: green;
  height: 100%;
  width: 100%;
}

.celula-morta {
  background-color: gray;
  height: 100%;
  width: 100%;
}
</style>
