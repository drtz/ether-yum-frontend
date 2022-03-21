<template>
    <div>
        <input v-model="state.address" @input="reset" v-on:keyup.enter="search" placeholder="contract address" />&nbsp;
        <button @click="search">Search Contracts</button>
    </div>
    <div v-if="state.isLoading">
        <span>Loading...</span>
    </div>
    <div v-if="state.timestamp != null">
        <label>Contract Create at Timestamp: </label>
        <span>{{state.timestamp}} ({{new Date(state.timestamp*1000)}})</span>
    </div>
    <ul v-if="state.errors != null">
        <li v-for="error in state.errors" :key="error.message">
            <label>Error: </label>
            <span>{{error.message}}</span>
        </li>
    </ul>
</template>

<script>
import { reactive, defineComponent } from "vue";
export default defineComponent({
  name: 'ContractSearch',
  setup() {
    const state = reactive({
        address: '',
        timestamp: null,
        isLoading: false,
        errors: null,
    });

    function reset() {
        this.isLoading = false;
        this.errors = null;
    }

    async function search() {
        state.errors = null;
        state.isLoading = true;

        // Poor-man's GraphQL query. Using Apollo here would be much cleaner.
        const body = { "query":`query { contract(address: "${state.address}") { createdInTransaction { timestamp } } }` };
        const response = await fetch('http://localhost:4000', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(body),
        });

        state.isLoading = false;

        const responseBody = await response.json();

        if (responseBody.errors) {
            state.errors = responseBody.errors;
        }

        if (responseBody.data && responseBody.data.contract) {
            state.timestamp = responseBody.data.contract.createdInTransaction.timestamp;
        }
    }

    return { state, reset, search };
  }
});
</script>


<style scoped>
</style>