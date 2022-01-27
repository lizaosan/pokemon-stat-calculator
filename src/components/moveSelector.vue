<template>
  <!-- <select v-model="move">
    <option v-for="(item, index) in parentMoves" :key="index"> {{ item.move.name }}</option>
  </select> -->
  <input class="moveInput" name="teleplay" list="teleplay_list"/>
  <datalist id="teleplay_list">
    <option v-for="(item, index) in testList" :key="index + 1" :value="item"/>
  </datalist>

</template>
<script>
  export default {
    name: "moveSelector",
    props: ["parentMoves"],
    data() {
      return {
        move: "try",
        moves: this.parentMoves,
        testList: [
          "招式測試 1",
          "招式測試 2",
          "招式測試 3",
          "招式測試 4",
          "招式測試 5",
        ]
      }
    }
  }
</script>

<style scoped>
  .moveInput {
    padding: 0;
    margin: 0;
    width: 7.5rem;
    margin-top: 0.1rem;
  }
</style>