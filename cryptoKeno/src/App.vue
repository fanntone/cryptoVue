<template>
  <div class="container">
    <table>
      <tr v-for="row in rows" :key="row">
        <td v-for="column in columns" :key="column">
          <button v-bind:class="{'selected': isSelected(row, column)}" v-bind:value="getValue(row, column)" v-on:click="selectButton(row, column)">
            {{ getValue(row, column) }}
          </button>
        </td>
      </tr>
    </table>
    <button v-on:click="submitSelection" :disabled="selectedButtons.length === 0 || selectedButtons.length > 10">Submit Selection</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      rows: 10,
      columns: 4,
      selectedButtons: []
    };
  },
  methods: {
    getValue(row, column) {
      return (row - 1) * this.columns + column;
    },
    isSelected(row, column) {
      return this.selectedButtons.includes(this.getValue(row, column));
    },
    selectButton(row, column) {
      const value = this.getValue(row, column);
      if (this.isSelected(row, column)) {
        this.selectedButtons = this.selectedButtons.filter((button) => button !== value);
      } else if (this.selectedButtons.length < 10){
        this.selectedButtons.push(value);
      }
    },
    submitSelection() {
      // Send post request with selected buttons data
      console.log("Selected buttons:", this.selectedButtons);
      this.selectedButtons = [];
    }
  }
};
</script>

<style>
table {
  border-collapse: collapse;
}
td {
  border: 1px solid black;
  padding: 5px;
}
button {
  width: 100%;
  height: 100%;
  background-color: white;
  border: none;
  font-size: 14px;
}
button:hover {
  background-color: lightgray;
}
button.selected {
  background-color: yellow;
}
.container {
  margin: 20px;
}
</style>
