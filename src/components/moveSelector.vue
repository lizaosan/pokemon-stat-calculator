<template>
  <select v-model="move">
    <option v-for="(item, index) in parentMoves" :key="index"> {{ item.move.name }}</option>
  </select>
</template>
<script>
  export default {
    name: "moveSelector",
    props: ["parentMoves"],
    data() {
      return {
        move: "try",
        moves: this.parentMoves,
      }
    }
  }
</script>

<style scoped>

</style>