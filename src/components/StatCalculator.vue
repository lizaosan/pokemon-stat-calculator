<template>
  <h1>Pokemon Stat Calculator</h1>
  <main class="container">
    <div class="input_wrapper">
      <div>
        <span>寶可夢</span>
        <select>
          <option>001</option>
          <option>002</option>
          <option>003</option>
          <option>004</option>
          <option>005</option>
          <option>006</option>
        </select>
      </div>
      <div>
        <span>等級</span><input type="number" min="1" max="100" />
      </div>
      <div>
        <span>性格</span>
        <select>
          <option>001</option>
          <option>002</option>
          <option>003</option>
          <option>004</option>
          <option>005</option>
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
          <td></td>
          <td><input type="number" min="0" max="31" /></td>
          <td><input type="number" min="0" max="252" /></td>
          <td></td>
        </tr>
        <tr>
          <td>攻擊</td>
          <td></td>
          <td><input type="number" min="0" max="31" /></td>
          <td><input type="number" min="0" max="252" /></td>
          <td></td>
        </tr>
        <tr>
          <td>防禦</td>
          <td></td>
          <td><input type="number" min="0" max="31" /></td>
          <td><input type="number" min="0" max="252" /></td>
          <td></td>
        </tr>
        <tr>
          <td>特攻</td>
          <td></td>
          <td><input type="number" min="0" max="31" /></td>
          <td><input type="number" min="0" max="252" /></td>
          <td></td>
        </tr>
        <tr>
          <td>特防</td>
          <td></td>
          <td><input type="number" min="0" max="31" /></td>
          <td><input type="number" min="0" max="252" /></td>
          <td></td>
        </tr>
        <tr>
          <td>速度</td>
          <td></td>
          <td><input type="number" min="0" max="31" /></td>
          <td><input type="number" min="0" max="252" /></td>
          <td></td>
        </tr>
      </tbody>
    </table>
  </main>

  <!-- <main class="container">
    <div class="input_wrapper">
      <div class="inputs">
        <div>
          <span>等級</span><input type="number" min="1" max="100" />
        </div>
        <div>
          <span>種族值</span><input type="number" min="1" max="500" />
        </div>
        <div>
          <span>個體值</span><input type="number" min="0" max="31" />
        </div>
        <div>
          <span>努力值</span><input type="number" min="0" max="252" />
        </div>
        <div><b>HP值</b><input type="number" readonly /></div>
      </div>
    </div>
    <div class="input_wrapper">
      <div class="inputs">
        <div>
          <span>等級</span><input type="number" min="1" max="100" />
        </div>
        <div>
          <span>種族值</span><input type="number" min="1" max="500" />
        </div>
        <div>
          <span>個體值</span><input type="number" min="0" max="31" />
        </div>
        <div>
          <span>努力值</span><input type="number" min="0" max="252" />
        </div>
        <div><b>HP值</b><input type="number" readonly /></div>
      </div>
      <div class="inputs">
        <div>
          <span>等級</span><input type="number" min="1" max="100" />
        </div>
        <div>
          <span>種族值</span><input type="number" min="1" max="500" />
        </div>
        <div>
          <span>個體值</span><input type="number" min="0" max="31" />
        </div>
        <div>
          <span>努力值</span><input type="number" min="0" max="252" />
        </div>
        <div>
          <span>性格補正</span>
          <form>
            <span><input value="1.1" name="nature" type="radio" />1.1</span>
            <span><input value="1" name="nature" type="radio" checked />1</span>
            <span><input value="0.9" name="nature" type="radio" />0.9</span>
          </form>
        </div>
        <div><b>能力值</b><input v-model="getStat" type="number" readonly /></div>
      </div>
    </div>
  </main> -->
</template>

<script>
  export default {
    data() {
      return {
        zhHantJsonUrl: "json/zh-hant.json",
        hpCalc: {
          level: 50,
          speciesStrength: 100,
          individualValues: 31,
          effortValue: 252,
        },
        statCalc: {
          level: 50,
          speciesStrength: 100,
          individualValues: 31,
          effortValue: 252,
          nature: 1
        }
      };
    },
    computed: {
      // getHP() {
      //     let that = this.hpCalc;
      //     return (
      //         Math.floor((Math.floor(that.speciesStrength * 2 + that.individualValues + that.effortValue / 4) * that.level) / 100) + 10 + that.level);
      // },
      // getStat() {
      //     let that = this.statCalc;
      //     return (
      //         Math.floor((Math.floor((Math.floor(that.speciesStrength * 2 + that.individualValues + that.effortValue / 4) * that.level) / 100) + 5) * that.nature)
      //     );
      // },
    },
  };
</script>

<style scoped>
  .container {
    display: flex;
    flex-direction: column;
    max-width: 50rem;
    margin: 0 auto;
  }

  .input_wrapper {
    width: 100%;
    margin: 0 auto;
    display: flex;
    flex: 1;
    justify-content: space-around;
  }

  /* .inputs {
    display: flex;
    flex-direction: row;
    flex: 1;
  }

  .inputs div {
    width: 100%;
    margin-bottom: 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .inputs input {
    width: 100%;
    text-align: center;
    width: 4rem;

  }

  .inputs div:last-child input {
    border: 0;
  }

  .inputs input::-webkit-outer-spin-button,
  .inputs input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  .inputs form {
    display: flex;
    flex-direction: column;
    text-align: left;
  }

  .inputs form input {
    width: 1.5rem;
  } */
</style>