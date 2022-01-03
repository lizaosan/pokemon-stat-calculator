<template>
  <h1>Pokemon Stat Calculator</h1>
  <main class="container">
    <div class="input_wrapper">
      <div>
        <span>寶可夢</span>
        <select v-model="selectedPM">
          <option v-for="(item, index) in zhHantName" :key="index" :value="index + 1">
            {{ 9 > index ? "00" + (index + 1) : 99 > index ? "0" + (index + 1) : index + 1 }} - {{ item }}</option>
        </select>
      </div>
      <div>
        <span>等級</span><input v-model="level" type="number" min="1" max="100" />
      </div>
      <div>
        <span>性格</span>
        <select v-model="nature">
          <option :value="0">不加不減</option>
          <option :value="12">+A-B 怕寂寞</option>
          <option :value="13">+A-C 固執</option>
          <option :value="14">+A-D 頑皮</option>
          <option :value="15">+A-S 勇敢</option>
          <option :value="21">+B-A 大膽</option>
          <option :value="23">+B-C 淘氣</option>
          <option :value="24">+B-D 樂天</option>
          <option :value="25">+B-S 悠閒</option>
          <option :value="31">+C-A 內斂</option>
          <option :value="32">+C-B 慢吞吞</option>
          <option :value="34">+C-D 馬虎</option>
          <option :value="35">+C-S 冷靜</option>
          <option :value="41">+D-A 溫和</option>
          <option :value="42">+D-B 溫順</option>
          <option :value="43">+D-C 慎重</option>
          <option :value="45">+D-S 自大</option>
          <option :value="51">+S-A 膽小</option>
          <option :value="52">+S-B 急躁</option>
          <option :value="53">+S-C 爽朗</option>
          <option :value="54">+S-D 天真</option>
        </select>
      </div>
    </div>
    <table>
      <thead>
        <tr>
          <th>項目</th>
          <th>種族值</th>
          <th>個體值</th>
          <th>努力值</th>
          <th>能力值</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>HP</td>
          <td>{{selectedStat.stats[0].base_stat}}</td>
          <td><input v-model="userIV.hp" type="number" min="0" max="31" /></td>
          <td><input v-model="userEV.hp" type="number" min="0" max="252" /></td>
          <td>{{getHP}}</td>
        </tr>
        <tr>
          <td>攻擊</td>
          <td>{{selectedStat.stats[1].base_stat}}</td>
          <td><input v-model="userIV.atk" type="number" min="0" max="31" /></td>
          <td><input v-model="userEV.atk" type="number" min="0" max="252" /></td>
          <td :class="{'text_red': naturePatch[0] == 1.1 , 'text_blue' : naturePatch[0] == 0.9}">{{getAtk}}</td>
        </tr>
        <tr>
          <td>防禦</td>
          <td>{{selectedStat.stats[2].base_stat}}</td>
          <td><input v-model="userIV.def" type="number" min="0" max="31" /></td>
          <td><input v-model="userEV.def" type="number" min="0" max="252" /></td>
          <td :class="{'text_red': naturePatch[1] == 1.1 , 'text_blue' : naturePatch[1] == 0.9}">{{getDef}}</td>
        </tr>
        <tr>
          <td>特攻</td>
          <td>{{selectedStat.stats[3].base_stat}}</td>
          <td><input v-model="userIV.spatk" type="number" min="0" max="31" /></td>
          <td><input v-model="userEV.spatk" type="number" min="0" max="252" /></td>
          <td :class="{'text_red': naturePatch[2] == 1.1 , 'text_blue' : naturePatch[2] == 0.9}">{{getSpAtk}}</td>
        </tr>
        <tr>
          <td>特防</td>
          <td>{{selectedStat.stats[4].base_stat}}</td>
          <td><input v-model="userIV.spdef" type="number" min="0" max="31" /></td>
          <td><input v-model="userEV.spdef" type="number" min="0" max="252" /></td>
          <td :class="{'text_red': naturePatch[3] == 1.1 , 'text_blue' : naturePatch[3] == 0.9}">{{getSpDef}}</td>
        </tr>
        <tr>
          <td>速度</td>
          <td>{{selectedStat.stats[5].base_stat}}</td>
          <td><input v-model="userIV.spd" type="number" min="0" max="31" /></td>
          <td><input v-model="userEV.spd" type="number" min="0" max="252" /></td>
          <td :class="{'text_red': naturePatch[4] == 1.1 , 'text_blue' : naturePatch[4] == 0.9}">{{getSpd}}</td>
        </tr>
      </tbody>
    </table>
    <small class="ev_details">
      努力值總計 <span>{{EVSum}}</span> / 剩餘努力值 <span>{{EVLast}}</span>
    </small>
    <div class="reset_btn">
      <button @click="resetAll">重置</button>
    </div>
  </main>
</template>

<script>
  export default {
    name: 'statCalculator',
    data() {
      return {
        zhHantJsonUrl: "json/zh-hant.json",
        zhHantName: [],
        selectedPM: "1",
        selectedStat: {
          stats: [{
              base_stat: 45
            },
            {
              base_stat: 49
            },
            {
              base_stat: 49
            },
            {
              base_stat: 65
            },
            {
              base_stat: 65
            },
            {
              base_stat: 45
            },
          ]
        },
        userIV: {
          hp: 31,
          atk: 31,
          def: 31,
          spatk: 31,
          spdef: 31,
          spd: 31
        },
        userEV: {
          hp: 0,
          atk: 0,
          def: 0,
          spatk: 0,
          spdef: 0,
          spd: 0
        },
        level: 50,
        nature: "0",
        naturePatch: [1, 1, 1, 1, 1],
      };
    },
    methods: {
      getzhHantNameJson(uri) {
        let vm = this;
        fetch(uri)
          .then(function (response) {
            return response.json();
          })
          .then(function (msg) {
            vm.zhHantName = msg;
          });
      },
      getSelectedStatJson(uri) {
        let vm = this;
        fetch(uri)
          .then(function (response) {
            return response.json();
          })
          .then(function (msg) {
            vm.selectedStat = msg;
          });
      },
      resetAll() {
        this.selectedPM = 1;
        this.level = 50;
        this.nature = "0";
        this.userIV = {
          hp: 31,
          atk: 31,
          def: 31,
          spatk: 31,
          spdef: 31,
          spd: 31
        };
        this.userEV = {
          hp: 0,
          atk: 0,
          def: 0,
          spatk: 0,
          spdef: 0,
          spd: 0
        }
      }
    },
    computed: {
      getHP() {
        let vm = this;
        return (
          Math.floor((Math.floor(vm.selectedStat.stats[0].base_stat * 2 + vm.userIV.hp + vm.userEV.hp / 4) * vm
            .level) / 100) + 10 + vm.level);
      },
      getAtk() {
        let vm = this;
        return (
          Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[1].base_stat * 2 + vm.userIV.atk + vm.userEV.atk /
            4) * vm.level) / 100) + 5) * vm.naturePatch[0])
        );
      },
      getDef() {
        let vm = this;
        return (
          Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[2].base_stat * 2 + vm.userIV.def + vm.userEV.def /
            4) * vm.level) / 100) + 5) * vm.naturePatch[1])
        );
      },
      getSpAtk() {
        let vm = this;
        return (
          Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[3].base_stat * 2 + vm.userIV.spatk + vm.userEV
            .spatk /
            4) * vm.level) / 100) + 5) * vm.naturePatch[2])
        );
      },
      getSpDef() {
        let vm = this;
        return (
          Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[4].base_stat * 2 + vm.userIV.spdef + vm.userEV
            .spdef /
            4) * vm.level) / 100) + 5) * vm.naturePatch[3])
        );
      },
      getSpd() {
        let vm = this;
        return (
          Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[5].base_stat * 2 + vm.userIV.spd + vm.userEV.spd /
            4) * vm.level) / 100) + 5) * vm.naturePatch[4])
        );
      },
      EVSum() {
        let vm = this;
        return (
          vm.userEV.hp + vm.userEV.atk + vm.userEV.def + vm.userEV.spatk + vm.userEV.spdef + vm.userEV.spd
        );
      },
      EVLast() {
        let vm = this;
        return (
          510 - (vm.userEV.hp + vm.userEV.atk + vm.userEV.def + vm.userEV.spatk + vm.userEV.spdef + vm.userEV.spd)
        );
      }
    },
    watch: {
      "selectedPM": {
        handler: function () {
          this.getSelectedStatJson('https://pokeapi.co/api/v2/pokemon/' + this.selectedPM);
        },
        immediate: true
      },
      "nature": {
        handler: function () {
          let array = this.nature.split("")
          for (let i = 0; i < this.naturePatch.length; i++) {
            if (i + 1 == array[0]) {
              this.naturePatch[i] = 1.1;
            } else if (i + 1 == array[1]) {
              this.naturePatch[i] = 0.9;
            } else {
              this.naturePatch[i] = 1;
            }
          }
        }
      },
      "userIV":{
        handler: function(val, oldVal){
          console.log(`new: ${val.hp}, old: ${oldVal.hp}`)
        },
        deep: true
        // 為什麼監聽起來怪怪的
      }
    },
    mounted() {
      this.getzhHantNameJson(this.zhHantJsonUrl);
    }
  };
</script>

<style scoped>

  h1 {
    font-size: 2rem;
    font-weight: bold;
  }


  input {
    text-align: center;
  }

  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  input[type=number] {
    -moz-appearance: textfield;
  }

  table {
    margin-top: 1rem;
  }

  tr {
    height: 1.5rem;
  }

  .container {
    display: flex;
    flex-direction: column;
    max-width: 50rem;
    margin: 1rem auto 0 auto;
  }

  .input_wrapper {
    width: 100%;
    margin: 0 auto;
    display: flex;
    flex: 1;
    justify-content: space-around;
  }

  .input_wrapper span {
    margin-right: .25rem;
  }



  .ev_details {
    margin-top: 1rem;
  }

  .reset_btn {
    margin-top: 1rem;
  }

  .text_red {
    color: #dc3545;
  }

  .text_blue {
    color: #007bff;
  }


</style>