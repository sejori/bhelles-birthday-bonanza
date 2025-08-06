<template>
  <div class="spinner-page pastel-bg">
    <h1 class="pastel-pink">Bhelle's Birthday Bonanza</h1>
    <label for="invitee" class="pastel-red">Select your name:</label>
    <select v-model="selectedInvitee" id="invitee" class="pastel-purple-border">
      <option disabled value="">-- Select --</option>
      <option v-for="invitee in invitees" :key="invitee" :value="invitee">
        {{ invitee }}
      </option>
    </select>
    <button :disabled="!selectedInvitee" @click="spin" class="pastel-purple-btn">Spin!</button>
    <div class="spinner-container">
      <!-- Spinner animation will go here -->
      <div v-if="spinning" class="spinner pastel-pink">Spinning...</div>
      <div v-else-if="result">
        <span :id="confettiId"></span>
        <h2 class="pastel-pink">{{ selectedInvitee }}, your character is:</h2>
        <p class="result pastel-red">{{ result }}</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useReward } from 'vue-rewards';

const invitees = [
  'Seb', 'Bhelle', 'Ammar', 'Bianca', 'Imogen', 'Jo', 'Justin', 'Lola',
  'Mylena', 'Renata', 'Rishika', 'Adriana', 'Elana', 'Greta', 'Gary'
];

const characters = [
  // 🇧🇷 Brazilian TV / Cinema Characters
  'Carminha (Avenida Brasil): Blonde wig, white outfit, sunglasses, rage.',
  'Nazaré Tedesco (Senhora do Destino): Floral blouse, haunted stare. Add meme math symbols.',
  'Joelma from Calypso: Platform boots, sequins, blonde hair drama.',
  'Chiquinha (Chaves): Red dress, big glasses, pigtails.',
  'Agostinho Carrara (A Grande Família): Loud shirt, chain, open chest.',
  'Jade (O Clone): Green belly dancer look, veil, henna tattoos. Iconic 2000s glam.',
  'Bebel (Paraíso Tropical): Leopard print, pink lipstick, chewing gum, "Ricardooo!"',
  'Xuxa (Planeta Xuxa Era): White boots, miniskirt, microphone — you are the show.',
  // 🇬🇧 UK TV / Film Characters
  'Cassie Ainsworth (Skins): Messy blonde hair, floral dress, vacant smile.',
  'Sharon or Tracey (Birds of a Feather): Animal print, Essex hair.',
  'Diana from The Crown (S4–S5): Revenge dress or 90s off-duty casual.',
  'Villanelle (Killing Eve): One of her fashion moments – eg. the pink tulle dress.',
  'Fleabag (Fleabag): Black jumpsuit, red lipstick, messy chic.',
  // 🇺🇸 US TV / Film Characters
  'Margot Tenenbaum (The Royal Tenenbaums): Lacoste dress, fur coat, eyeliner.',
  'Moira Rose (Schitt’s Creek): Wig, black & white couture, over-the-top accessories.',
  'Tom Cruise (Risky Business): Long white shirt, underwear, socks and sunglasses.',
  'Barbie (Barbie Movie): Pink everything, rollerblades or cowgirl version.',
  'Ken: Sleeveless shirt, sunglasses, "Kenergy".',
  'Wednesday Addams: Black dress, braids, deadpan expression.',
  'Elle Woods (Legally Blonde): Pink suit, chihuahua bag, "What, like it\'s hard?"',
  'Regina George (Mean Girls): Tank top with holes, mini skirt.',
  'Maddy Perez (Euphoria): Strappy crop top, rhinestones, strong brows.',
  'Rachel Green (Friends): 90s jeans, apron (Central Perk), layered hair.',
  // 🎤 Pop Culture Queens
  'Paris Hilton 2004: Low-rise mini skirt, slogan tee, tiny dog.',
  'Anitta – “EnvolSchitt’sSportswear, hoop earrings, body oil sheen.'
];

// Hardcoded mapping for now (1:1)
const assignments: Record<string, string> = {};
invitees.forEach((invitee, i) => {
  assignments[invitee] = characters[i % characters.length];
});

const selectedInvitee = ref('');
const spinning = ref(false);
const result = ref<string | null>(null);

const confettiId = 'confetti-reward';
const { reward: confettiReward } = useReward(confettiId, 'confetti', {
  elementCount: 120,
  spread: 120,
  startVelocity: 30,
  elementSize: 10,
  colors: [
    '#FF0080', '#7928CA', '#0078FF', '#00FFB8', '#FFD600', '#FF8C00', '#FF0000', '#00FF00', '#00FFFF', '#FF00FF'
  ]
});

function spin() {
  if (!selectedInvitee.value) return;
  spinning.value = true;
  result.value = null;
  setTimeout(() => {
    result.value = assignments[selectedInvitee.value];
    spinning.value = false;
    setTimeout(() => {
      confettiReward();
    }, 100); // slight delay to ensure DOM update
  }, 2000); // Simulate spin duration
}
</script>

<style scoped>
.pastel-bg {
  background: #f3f4f8;
  min-height: 100vh;
}
.spinner-page {
  max-width: 500px;
  margin: 2rem auto;
  text-align: center;
  border-radius: 18px;
  padding-bottom: 2rem;
}
.pastel-pink {
  color: #ffb6d5;
}
.pastel-red {
  color: #ff7f7f;
}
.pastel-purple-btn {
  background: #d1b3ff;
  color: #fff;
  font-weight: bold;
  box-shadow: 0 0 10px #f3e6ff, 0 0 20px #f3e6ff;
  border: none;
  border-radius: 8px;
  padding: 0.5rem 1.5rem;
  font-size: 1.1rem;
  cursor: pointer;
  margin-bottom: 1rem;
  transition: background 0.3s, box-shadow 0.3s;
}
.pastel-purple-btn:hover:not(:disabled) {
  background: #b39ddb;
  box-shadow: 0 0 20px #e0cfff, 0 0 40px #e0cfff;
}
.pastel-purple-border {
  border: 2px solid #d1b3ff;
  border-radius: 8px;
  padding: 0.5rem;
  font-size: 1rem;
  margin: 1rem;
}
select {
  font-size: 1rem;
}
button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
.spinner-container {
  margin-top: 2rem;
  min-height: 60px;
}
.spinner {
  font-size: 1.5rem;
  animation: spin 1s linear infinite;
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.result {
  font-size: 1.3rem;
  font-weight: bold;
  margin-top: 1rem;
}
</style> 