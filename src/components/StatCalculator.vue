<template>
  <h1>Pokemon Stat Calculator</h1>
  <h2>HP值計算機</h2>
  <div class="inputs">
    <div>
      <span>等級</span><input v-model="level" type="number" min="1" max="100" />
    </div>
    <div>
      <span>種族值</span
      ><input v-model="speciesStrength" type="number" min="1" max="500" />
    </div>
    <div>
      <span>個體值</span
      ><input v-model="individualValues" type="number" min="0" max="31" />
    </div>
    <div>
      <span>努力值</span
      ><input v-model="effortValue" type="number" min="0" max="252" />
    </div>
    <div><b>能力值</b><input v-model="getStatus" type="number" readonly /></div>
  </div>
  <small>Last Edited by: Lizaosan 20211214 03:40PM</small>
</template>

<script>
export default {
  data() {
    return {
      level: 50,
      speciesStrength: 100,
      individualValues: 31,
      effortValue: 252,
    };
  },
  computed: {
    getStatus() {
      return (
        Math.floor(
          (Math.floor(
            this.speciesStrength * 2 +
              this.individualValues +
              this.effortValue / 4
          ) *
            this.level) /
            100
        ) +
        10 +
        this.level
      );
    },
  },
};
</script>

<style scoped>
.inputs {
  display: flex;
  flex-direction: column;
  width: 4rem;
  margin: 0 auto;
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
}

.inputs div:last-child input {
  border: 0;
}

.inputs input::-webkit-outer-spin-button,
.inputs input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>