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
        <span>等級</span>
        <!-- <input v-model="level" type="number" min="1" max="100" /> -->
        <select v-model="level">
           <option v-for="(item, index) in 50" :key="index + 1">{{index + 1 }}</option>
        </select>
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
          <individualValue :parent-msg="userIV[0]" iv-order="0" @update="updateIV" :key="resetFrequency"></individualValue>
          <effortValue :parent-msg="userEV[0]" ev-order="0" @update="updateEV" :key="resetFrequency"></effortValue>
          <td>{{getHP}}</td>
        </tr>
        <tr>
          <td>攻擊</td>
          <td>{{selectedStat.stats[1].base_stat}}</td>
          <individualValue :parent-msg="userIV[1]" iv-order="1" @update="updateIV" :key="resetFrequency"></individualValue>
          <effortValue :parent-msg="userEV[1]" ev-order="1" @update="updateEV" :key="resetFrequency"></effortValue>
          <td :class="{'text_red': naturePatch[0] == 1.1 , 'text_blue' : naturePatch[0] == 0.9}">{{getAtk}}</td>
        </tr>
        <tr>
          <td>防禦</td>
          <td>{{selectedStat.stats[2].base_stat}}</td>
          <individualValue :parent-msg="userIV[2]" iv-order="2" @update="updateIV" :key="resetFrequency"></individualValue>
          <effortValue :parent-msg="userEV[2]" ev-order="2" @update="updateEV" :key="resetFrequency"></effortValue>
          <td :class="{'text_red': naturePatch[1] == 1.1 , 'text_blue' : naturePatch[1] == 0.9}">{{getDef}}</td>
        </tr>
        <tr>
          <td>特攻</td>
          <td>{{selectedStat.stats[3].base_stat}}</td>
          <individualValue :parent-msg="userIV[3]" iv-order="3" @update="updateIV" :key="resetFrequency"></individualValue>
          <effortValue :parent-msg="userEV[3]" ev-order="3" @update="updateEV" :key="resetFrequency"></effortValue>
          <td :class="{'text_red': naturePatch[2] == 1.1 , 'text_blue' : naturePatch[2] == 0.9}">{{getSpAtk}}</td>
        </tr>
        <tr>
          <td>特防</td>
          <td>{{selectedStat.stats[4].base_stat}}</td>
          <individualValue :parent-msg="userIV[4]" iv-order="4" @update="updateIV" :key="resetFrequency"></individualValue>
          <effortValue :parent-msg="userEV[4]" ev-order="4" @update="updateEV" :key="resetFrequency"></effortValue>
          <td :class="{'text_red': naturePatch[3] == 1.1 , 'text_blue' : naturePatch[3] == 0.9}">{{getSpDef}}</td>
        </tr>
        <tr>
          <td>速度</td>
          <td>{{selectedStat.stats[5].base_stat}}</td>
          <individualValue :parent-msg="userIV[5]" iv-order="5" @update="updateIV" :key="resetFrequency"></individualValue>
          <effortValue :parent-msg="userEV[5]" ev-order="5" @update="updateEV" :key="resetFrequency"></effortValue>
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
  import individualValue from "./IndividualValue.vue";
  import effortValue from "./EffortValue.vue";

  export default {
    name: 'statCalculator',
    components: {
      individualValue, effortValue
    },
    data() {
      return {
        zhHantJsonUrl: "json/zh-hant.json",
        zhHantName: [],
        selectedPM: "1",
        selectedStat: {
          stats: [{base_stat: 45},{base_stat: 49},{base_stat: 49},{base_stat: 65},{base_stat: 65},{base_stat: 45},]
        },
        userIV: [31, 31, 31, 31, 31, 31],
        userEV: [0, 0, 0, 0, 0, 0],
        level: 50,
        nature: "0",
        naturePatch: [1, 1, 1, 1, 1],
        resetFrequency: 0,
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
        this.level = 50;
        this.nature = "0";
        this.$data.userIV = this.$options.data().userIV;
        this.$data.userEV = this.$options.data().userEV;
        this.resetFrequency += 1;
      },
      updateIV(val, order) {
        this.userIV[order] = val;
      },
      updateEV(val, order) {
        this.userEV[order] = val;
      },
    },
    computed: {
      getHP() {
        let vm = this;
        let result = null;
        result = vm.userIV[0] === '' || vm.userEV[0] === '' ? "錯誤" : Math.floor((Math.floor(vm.selectedStat.stats[0].base_stat * 2 + (vm.userIV[0]*1) + (vm.userEV[0]*1) / 4) * vm.level) / 100) + 10 + (vm.level * 1);
        return result;
      },
      getAtk() {
        let vm = this;
        let result = null;
        result = vm.userIV[1] === '' || vm.userEV[1] === '' ? "錯誤" : Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[1].base_stat * 2 + (vm.userIV[1]*1) + (vm.userEV[1]*1) / 4) * vm.level) / 100) + 5) * vm.naturePatch[0]);
        return result;
      },
      getDef() {
        let vm = this;
        let result = null;
        result = vm.userIV[2] === '' || vm.userEV[2] === '' ? "錯誤" : Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[2].base_stat * 2 + (vm.userIV[2]*1) + (vm.userEV[2]*1) / 4) * vm.level) / 100) + 5) * vm.naturePatch[1]);
        return result;
      },
      getSpAtk() {
        let vm = this;
        let result = null;
        result = vm.userIV[3] === '' || vm.userEV[3] === '' ? "錯誤" : Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[3].base_stat * 2 + (vm.userIV[3]*1) + (vm.userEV[3]*1) / 4) * vm.level) / 100) + 5) * vm.naturePatch[2]);
        return result;
      },
      getSpDef() {
        let vm = this;
        let result = null;
        result = vm.userIV[4] === '' || vm.userEV[4] === '' ? "錯誤" : Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[4].base_stat * 2 + (vm.userIV[4]*1) + (vm.userEV[4]*1) / 4) * vm.level) / 100) + 5) * vm.naturePatch[3]);
        return result;
      },
      getSpd() {
        let vm = this;
        let result = null;
        result = vm.userIV[5] === '' || vm.userEV[5] === '' ? "錯誤" : Math.floor((Math.floor((Math.floor(vm.selectedStat.stats[5].base_stat * 2 + (vm.userIV[5]*1) + (vm.userEV[5]*1) / 4) * vm.level) / 100) + 5) * vm.naturePatch[4]);
        return result;
      },
      EVSum() {
        let vm = this;
        return (
          (vm.userEV[0]*1) + (vm.userEV[1]*1) + (vm.userEV[2]*1) + (vm.userEV[3]*1) + (vm.userEV[4]*1) + (vm.userEV[5]*1)
        );
      },
      EVLast() {
        let vm = this;
        return (
          510 - ((vm.userEV[0]*1) + (vm.userEV[1]*1) + (vm.userEV[2]*1) + (vm.userEV[3]*1) + (vm.userEV[4]*1) + (vm.userEV[5]*1))
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
      "level": {
        handler: function (val) {
          val < 0 && (this.level = 1)
					val > 100 && (this.level = 100);
        }
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

	input:focus {
		outline: none;
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