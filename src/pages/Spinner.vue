<template>
  <div class="spinner-page">
    <h1>Costume Spinner</h1>
    <label for="invitee">Select your name:</label>
    <select v-model="selectedInvitee" id="invitee">
      <option disabled value="">-- Select --</option>
      <option v-for="invitee in invitees" :key="invitee" :value="invitee">
        {{ invitee }}
      </option>
    </select>
    <button :disabled="!selectedInvitee" @click="spin">Spin!</button>
    <div class="spinner-container">
      <!-- Spinner animation will go here -->
      <div v-if="spinning" class="spinner">Spinning...</div>
      <div v-else-if="result">
        <h2>{{ selectedInvitee }}, your character is:</h2>
        <p class="result">{{ result }}</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const invitees = [
  'Seb', 'Bhelle', 'Ammar', 'Bianca', 'Imogen', 'Jo', 'Justin', 'Lola',
  'Mylena', 'Renata', 'Rishika', 'Adriana', 'Elana', 'Greta', 'Gary'
];

const characters = [
  'Harry Potter', 'Darth Vader', 'Wonder Woman', 'Sherlock Holmes', 'Mickey Mouse',
  'Batman', 'Elsa (Frozen)', 'Indiana Jones', 'James Bond', 'Homer Simpson',
  'Forrest Gump', 'Daenerys Targaryen', 'The Joker', 'Spock', 'Lara Croft'
];

// Hardcoded mapping for now (1:1)
const assignments: Record<string, string> = {};
invitees.forEach((invitee, i) => {
  assignments[invitee] = characters[i % characters.length];
});

const selectedInvitee = ref('');
const spinning = ref(false);
const result = ref<string | null>(null);

function spin() {
  if (!selectedInvitee.value) return;
  spinning.value = true;
  result.value = null;
  setTimeout(() => {
    result.value = assignments[selectedInvitee.value];
    spinning.value = false;
  }, 2000); // Simulate spin duration
}
</script>

<style scoped>
.spinner-page {
  max-width: 400px;
  margin: 2rem auto;
  text-align: center;
}
select {
  margin: 1rem;
  padding: 0.5rem;
}
button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
}
.spinner-container {
  margin-top: 2rem;
  min-height: 60px;
}
.spinner {
  font-size: 1.5rem;
  color: #888;
  animation: spin 1s linear infinite;
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.result {
  font-size: 1.3rem;
  font-weight: bold;
  color: #2a7;
}
</style> 