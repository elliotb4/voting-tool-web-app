<template>
  <div class="ResultTable">
    <h1>PrefLib Election Results</h1>
    <table>
      <tr>
        <th class="top">No. of Candidates</th>
        <td>{{ results.candidates }}</td>
      </tr>
      <tr>
        <th>Borda Count</th>
        <td>{{ results.borda }}</td>
      </tr>
      <tr>
        <th>Instant Runoff</th>
        <td>{{ results.irv }}</td>
      </tr>
      <tr>
        <th>Single Transferrable Vote</th>
        <td>{{ results.stv }}</td>
      </tr>
      <tr>
        <th>Plurality</th>
        <td>{{ results.plurality }}</td>
      </tr>
      <tr>
        <th>Condorcet Winner</th>
        <td>{{ results.condorcet }}</td>
      </tr>
    </table>
    <div class="select">
      <label for="dataset">Dataset: </label>
      <input
        v-model.number="dataset"
        @change="fetchResultData(dataset)"
        type="number"
        id="dataset"
        name="dataset"
        min="1"
        max="87"
      />
    </div>
  </div>
  <div class="efficiencyTable">
    <h2>Condorcet Efficiencies</h2>
    <table>
      <tr class="top">
        <th>Method</th>
        <th>Probability</th>
      </tr>
      <tr>
        <th>Modified Borda</th>
        <td>{{ efficiencies[0] }}</td>
      </tr>
      <tr>
        <th>Averaged Borda</th>
        <td>{{ efficiencies[1] }}</td>
      </tr>
      <tr>
        <th>IRV</th>
        <td>{{ efficiencies[2] }}</td>
      </tr>
      <tr>
        <th>Plurality</th>
        <td>{{ efficiencies[3] }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      results: [],
      dataset: 1,
      efficiencies: [],
      url: "http://localhost:8080/preflib",
    };
  },
  mounted() {
    this.fetchResultData(this.dataset);
    this.fetchEfficiencyData();
  },
  methods: {
    fetchResultData(dataset) {
      axios
        .post(this.url, {
          set: dataset,
        })
        .then((res) => {
          // console.log(res.data);
          this.results = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    fetchEfficiencyData() {
      axios
        .get(this.url)
        .then((res) => {
          this.efficiencies = res.data.efficiency;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style scoped>
input {
  background-color: grey;
  color: black;
  border: 1px solid black;
  padding: auto;
}

h1 {
  font-size: 45px;
  text-align: center;
}

h2 {
  text-align: center;
}

table {
  margin-left: auto;
  margin-right: auto;
}

table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}

th,
td {
  padding: 5px;
  text-align: center;
  background-color: gray;
  color: black;
}

th {
  font-weight: bold;
}

.select {
  text-align: center;
}

.top {
  text-decoration-line: underline;
  text-decoration-color: black;
}

td {
  text-decoration-line: none !important;
}
</style>
