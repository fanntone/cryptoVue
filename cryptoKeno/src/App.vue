<template>
  <div>
    <table>
      <tr v-for="i in 4" :key="i">
        <td v-for="j in 10" :key="10*(i-1)+j">
          <button :class="{ selected: isSelected(10*(i-1)+j) }" @click="selectButton(10*(i-1)+j)">{{ 10*(i-1)+j }}</button>
        </td>
      </tr>
    </table>
    <button @click="submitSelection" :disabled="selectedButtons.length === 0">Submit Selection</button>
    <div v-if="win">
      <h2>You win {{ win }}</h2>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buttons: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40],
      selectedButtons: [],
      win: null
    }
  },
  methods: {
    selectButton(value) {
      if (this.selectedButtons.includes(value)) {
        this.selectedButtons.splice(this.selectedButtons.indexOf(value), 1);
      } else if (this.selectedButtons.length < 10) {
        this.selectedButtons.push(value);
      }
    },
    isSelected(value) {
      return this.selectedButtons.includes(value);
    },
    submitSelection() {
      if (this.selectedButtons.length > 0) {
        const data = JSON.stringify({ selectedButtons: this.selectedButtons });
        fetch('http://127.0.0.1:8080/crypto', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: data
        })
        .then(response => {
          if (response.ok) {
            return response.json();
          }
          throw new Error('Network response was not ok.');
        })
        .then(json => {
          console.log('Response:', json);
          this.win = json.win;
          this.selectedButtons = [];
        })
        .catch(error => console.error('Error:', error));
      }
    }
  }
}
</script>

<style>
.selected {
  background-color: yellow;
}
</style>
