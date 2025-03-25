<template>
  <div class="cart">
    <div class="input-group">
      <input v-model="noviNaziv" placeholder="Naziv proizvoda" />
      <input type="number" v-model.number="novaCijena" min="0" placeholder="Cijena (€)" />
      <button @click="dodajProizvod" :disabled="!noviNaziv.trim()">Dodaj artikl</button>
    </div>

    <h2> Košarica </h2>

    <p v-if="kosarica.length === 0" class="empty">Košarica je prazna!</p>

    <table v-else class="cart-table">
      <thead>
        <tr>
          <th>Naziv</th>
          <th>Količina</th>
          <th>Cijena</th>
          <th>Ukupno</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(proizvod, index) in kosarica" :key="proizvod.naziv">
          <td>{{ proizvod.naziv }}</td>
          <td class="quantity-controls">
            <button @click="smanjiKolicinu(index)">-</button>
            <span>{{ proizvod.kolicina }}</span>
            <button @click="povecajKolicinu(index)">+</button>
          </td>
          <td>{{ proizvod.cijena.toFixed(2) }} €</td>
          <td>{{ (proizvod.kolicina * proizvod.cijena).toFixed(2) }} €</td>
          <td><button class="remove" @click="ukloniProizvod(index)">Ukloni</button></td>
        </tr>
      </tbody>
    </table>

    <h3 v-if="kosarica.length > 0"> Ukupno: <span class="total">{{ ukupno.toFixed(2) }} €</span></h3>
  </div>
</template>


<script setup>
import { ref, computed } from 'vue';

const noviNaziv = ref('');
const novaCijena = ref(0);
const kosarica = ref([]);


const dodajProizvod = () => {
  const naziv = noviNaziv.value.trim().toLowerCase();
  const cijena = novaCijena.value;

  if (!naziv) return;

  if (cijena < 0) {
    alert('Cijena ne može biti negativna!');
    return;
  }

  const index = kosarica.value.findIndex(
    p => p.naziv.toLowerCase() === naziv
  );

  if (index !== -1) {
    if (kosarica.value[index].cijena !== cijena) {
      alert('Proizvod s tim imenom već postoji s drugom cijenom.');
      return;
    }
    kosarica.value[index].kolicina++;
  } else {
    kosarica.value.push({
      naziv,
      cijena,
      kolicina: 1,
    });
  }

  noviNaziv.value = '';
  novaCijena.value = 0;
};

const povecajKolicinu = (index) => {
  kosarica.value[index].kolicina++;
};

const smanjiKolicinu = (index) => {
  if (kosarica.value[index].kolicina > 1) {
    kosarica.value[index].kolicina--;
  } else {
    ukloniProizvod(index);
  }
};

const ukloniProizvod = (index) => {
  kosarica.value.splice(index, 1);
};

const ukupno = computed(() =>
  kosarica.value.reduce((sum, p) => sum + p.kolicina * p.cijena, 0)
);

</script>

<style scoped>
.cart {
  background-color: white;
  padding: 1.5rem;
  border-radius: 8px;
}

.input-group {
  display: flex;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.input-group input {
  flex: 1;
  padding: 0.5rem;
  border: 1px solid #151414;
  border-radius: 4px;
}

.input-group button {
  padding: 0.5rem 1rem;
  background-color: #008cba;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.input-group button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.empty {
  font-style: italic;
  color: #777;
}

.cart-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
}

.cart-table th,
.cart-table td {
  padding: 0.75rem;
  text-align: center;
  border-bottom: 1px solid #eee;
}

.quantity-controls {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.quantity-controls button {
  padding: 0.2rem 0.5rem;
  background-color: #93baeb;
  border: 1px solid #ccc;
  cursor: pointer;
  border-radius: 4px;
}

.remove {
  border: 2px ;
  color: rgb(240, 5, 52);
  background: none;
  border: none;
  cursor: pointer;
}

.total {
  color: green;
  font-weight: bold;
  font-size: 1.2em;
}
</style>
