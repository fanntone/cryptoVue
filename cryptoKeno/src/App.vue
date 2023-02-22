<template>
  <div>
    <table>
      <tr v-for="(row, rowIndex) in buttonRows" :key="rowIndex">
        <td v-for="(button, buttonIndex) in row" :key="buttonIndex">
          <button :class="{ selected: selectedButtons.includes(button) }" @click="toggleSelection(button)">
            {{ button }}
          </button>
        </td>
      </tr>
    </table>
    <form @submit.prevent="submitBet">
      <label for="amount">Bet Amount:</label>
      <input type="number" id="amount" v-model="betAmount" step="0.01" min="0">
      <button type="submit" :disabled="selectedButtons.length === 0 || selectedButtons.length > 10">Submit</button>
    </form>
  </div>
</template>

<script>
// import { looseIndexOf } from '@vue/shared';

export default {
  data() {
    return {
      buttonRows: [
        [1, 2, 3, 4],
        [5, 6, 7, 8],
        [9, 10, 11, 12],
        [13, 14, 15, 16],
        [17, 18, 19, 20],
        [21, 22, 23, 24],
        [25, 26, 27, 28],
        [29, 30, 31, 32],
        [33, 34, 35, 36],
        [37, 38, 39, 40],
      ],
      selectedButtons: [],
      betAmount: 0,
    };
  },
  methods: {
    toggleSelection(button) {
      if (this.selectedButtons.includes(button)) {
        this.selectedButtons.splice(this.selectedButtons.indexOf(button), 1);
      } else {
        this.selectedButtons.push(button);
      }
    },
    submitBet() {
      const data = {
        // selectedFields: this.selectedButtons,
        // betAmount: this.betAmount,
        selectedFields: [1, 2, 3],
        betAmount: 0.05
      };
      fetch("http://localhost:5566/cryptokeon", {
        method: "POST",
        body: JSON.stringify(data),
        headers: {
          "Content-Type": "application/json",
        },
        credentials: "include", // 加入這個屬性
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          alert(`You won ${data.win}!`);
          this.selectedButtons = [];
          this.betAmount = 0;
        })
        .catch((error) => {
          console.error("Error:", error);
          alert("Error occurred while processing your bet!");
        });
    },
  },
};
</script>

<style scoped>
table {
  border-collapse: collapse;
}

td {
  padding: 0;
}

button {
  width: 100%;
  height: 100%;
  font-size: 14px;
  border: none;
  background-color: #f7f7f7;
  cursor: pointer;
}

.selected {
  background-color: #1abc9c;
  color: white;
}
</style>
