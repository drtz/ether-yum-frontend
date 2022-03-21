<template>
    <div>
        <input v-model="address" @input="timestamp=null" placeholder="contract address" />
        <button @click="search(address)">Search Contracts</button>
    </div>
    <div v-if="timestamp != null">
        <label>Contract Timestamp:</label>
        <span>{{timestamp}}</span>
    </div>
</template>

<script>
export default {
  name: 'ContractSearch',
  data() {
      return {
          address: '',
          timestamp: null,
      }
  },
  methods: {
    async search(address) {
        // Poor-man's GraphQL query. Using Apollo here would be much cleaner.
        const body = { "query":`query { contract(address: "${address}") { createdInTransaction { timestamp } } }` };
        const response = await fetch('http://localhost:4000', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(body),
        });

        const responseBody = await response.json();
        if (responseBody.data && responseBody.data.contract) {
            this.timestamp = responseBody.data.contract.createdInTransaction.timestamp;
        }
    }
  }
}
</script>


<style scoped>
</style>