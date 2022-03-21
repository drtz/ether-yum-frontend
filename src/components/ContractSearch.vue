<template>
    <div>
        <input v-model="address" @input="reset" v-on:keyup.enter="search" placeholder="contract address" />
        <button @click="search">Search Contracts</button>
    </div>
    <div v-if="isLoading">
        <span>Loading...</span>
    </div>
    <div v-if="timestamp != null">
        <label>Contract Timestamp:</label>
        <span>{{timestamp}}</span>
    </div>
    <ul v-if="errors != null">
        <li v-for="error in errors" :key="error.message">
            <label>Error:</label>
            <span>{{error.message}}</span>
        </li>
    </ul>
</template>

<script>
export default {
  name: 'ContractSearch',
  data() {
      return {
          address: '',
          timestamp: null,
          isLoading: false,
          errors: null,
      }
  },
  methods: {
    reset() {
        this.isLoading = false;
        this.errors = null;
    },

    async search() {
        this.errors = null;
        this.isLoading = true;

        // Poor-man's GraphQL query. Using Apollo here would be much cleaner.
        const body = { "query":`query { contract(address: "${this.address}") { createdInTransaction { timestamp } } }` };
        const response = await fetch('http://localhost:4000', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(body),
        });

        this.isLoading = false;

        const responseBody = await response.json();

        if (responseBody.errors) {
            this.errors = responseBody.errors;
        }

        if (responseBody.data && responseBody.data.contract) {
            this.timestamp = responseBody.data.contract.createdInTransaction.timestamp;
        }
    }
  }
}
</script>


<style scoped>
</style>