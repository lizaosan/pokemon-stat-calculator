<template>
    <h1>Pokemon Stat Calculator</h1>
    <div class="input_wrapper">
    <div class="inputs">
        <h2>HP計算機</h2>
        <div>
        <span>等級</span><input v-model="hpCalc.level" type="number" min="1" max="100" />
        </div>
        <div>
        <span>種族值</span><input v-model="hpCalc.speciesStrength" type="number" min="1" max="500" />
        </div>
        <div>
        <span>個體值</span><input v-model="hpCalc.individualValues" type="number" min="0" max="31" />
        </div>
        <div>
        <span>努力值</span><input v-model="hpCalc.effortValue" type="number" min="0" max="252" />
        </div>
        <div><b>HP值</b><input v-model="getHP" type="number" readonly /></div>
    </div>
    <div class="inputs">
        <h2>能力計算機</h2>
        <div>
        <span>等級</span><input v-model="statCalc.level" type="number" min="1" max="100" />
        </div>
        <div>
        <span>種族值</span><input v-model="statCalc.speciesStrength" type="number" min="1" max="500" />
        </div>
        <div>
        <span>個體值</span><input v-model="statCalc.individualValues" type="number" min="0" max="31" />
        </div>
        <div>
        <span>努力值</span><input v-model="statCalc.effortValue" type="number" min="0" max="252" />
        </div>
        <div>
        <span>性格補正</span>
        <form>
            <span><input v-model="statCalc.nature" value="1.1" name="nature" type="radio"/>1.1</span>
            <span><input v-model="statCalc.nature" value="1" name="nature" type="radio" checked/>1</span>
            <span><input v-model="statCalc.nature" value="0.9" name="nature" type="radio"/>0.9</span>
        </form>
        </div>
        <div><b>能力值</b><input v-model="getStat" type="number" readonly /></div>
    </div>
  </div>
  <small>Last Edited by: Lizaosan 20211214 04:30PM</small>
</template>

<script>
export default {
  data() {
    return {
        hpCalc:{
            level: 50,
            speciesStrength: 100,
            individualValues: 31,
            effortValue: 252,
        },
        statCalc:{
            level: 50,
            speciesStrength: 100,
            individualValues: 31,
            effortValue: 252,
            nature: 1
        }
    };
  },
  computed: {
    getHP() {
        return (
            Math.floor((Math.floor(this.hpCalc.speciesStrength * 2 + this.hpCalc.individualValues + this.hpCalc.effortValue / 4) * this.hpCalc.level) / 100) + 10 + this.hpCalc.level);
    },
    getStat() {
        return (
            Math.floor((Math.floor((Math.floor(this.statCalc.speciesStrength * 2 + this.statCalc.individualValues + this.statCalc.effortValue / 4) * this.statCalc.level) / 100) + 5) * this.statCalc.nature)
        );
    },
  },
};
</script>

<style scoped>

.input_wrapper {
    max-width: 50rem;
    margin: 0 auto;
    display: flex;
    flex-direction: row;
}
.inputs {
  display: flex;
  flex-direction: column;
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
}

small {
    display: block;
    position: fixed;
    bottom: 0;
    width: 100%;
    text-align: center;
    line-height: 2rem;
}
</style>