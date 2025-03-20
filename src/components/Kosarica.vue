<script setup>
import { ref, computed } from "vue";

const slike = {
  Jabuka: "https://www.svgrepo.com/show/530203/apple.svg",
  Mrkva: "https://www.svgrepo.com/show/530216/carrot.svg",
  Sir: "https://www.svgrepo.com/show/530219/cake.svg",
  Kruh: "https://www.svgrepo.com/show/530223/bread.svg"
};


const proizvodi = [
  { naziv: "Jabuka", cijena: 0.25 },
  { naziv: "Mrkva", cijena: 0.12 },
  { naziv: "Kruh", cijena: 2.0 },
  { naziv: "Sir", cijena: 4.48 }
];

const korisnik = ref({
    jeAdmin: true, 
  osobni_podaci: {
    ime: "",
    prezime: "",
    adresa: {
      grad: "",
      ulica: "",
      broj: ""
    },
    broj_telefona: ""
  },
  kosarica: []
});

setTimeout(() => {
  korisnik.value = {
    osobni_podaci: {
      ime: "Marko",
      prezime: "Krivić",
      adresa: {
        grad: "Pula",
        ulica: "Veruda",
        broj: 32
      },
      broj_telefona: "+091-123-456"
    },

  kosarica: [
    { naziv: "Jabuka", količina: 4 },
    { naziv: "Mrkva", količina: 12 },
    { naziv: "Sir", količina: 1 },
    { naziv: "Kruh", količina: 3 }
    ]
  };
}, 100);

const dohvatiCijenu = (naziv) => {
  const proizvod = proizvodi.find((p) => p.naziv === naziv);
  return proizvod ? proizvod.cijena : 0;
};

const sveukupnaCijena = computed(() => {
  return korisnik.value.kosarica.reduce(
    (total, item) => total + dohvatiCijenu(item.naziv) * item.količina,
    0
  );
});

const najskupljaStavka = computed(() => {
  return korisnik.value.kosarica.reduce((najskuplja, item) => {
    const cijena = dohvatiCijenu(item.naziv) * item.količina;
    return cijena > najskuplja.cijena ? { naziv: item.naziv, cijena } : najskuplja;
  }, { naziv: "", cijena: 0 }).naziv;
});
</script>

<template>

<div class="korisnik">
  <h2>Korisnički podaci</h2>
  <p><strong>Ime:</strong> <span :style="{ color: korisnik.jeAdmin ? '#007bff' : 'black'  }">
    {{ korisnik.osobni_podaci.ime }} {{ korisnik.osobni_podaci.prezime }}
  </span></p>
  <p><strong>Adresa:</strong> <span :style="{ color: korisnik.jeAdmin ? '#007bff' : 'black' }">
    {{ korisnik.osobni_podaci.adresa.ulica }} {{ korisnik.osobni_podaci.adresa.broj }}, {{ korisnik.osobni_podaci.adresa.grad }}
  </span></p>
  <p><strong>Telefon:</strong> <span :style="{ color: korisnik.jeAdmin ? '#007bff' : 'black' }">
    {{ korisnik.osobni_podaci.broj_telefona }}
  </span></p>
</div>

  <div class="kosarica-container">
    <h2>Košarica</h2>

    <div class="stavka" :class="{ 'najskuplja': najskupljaStavka === 'Jabuka' }">
      <img :src="slike.Jabuka" alt="Jabuka">
      <div class="info">
        <p><strong>Jabuka</strong></p>
        <p>Cijena: €0.25 | Količina: 4</p>
        <p>Ukupno: €1.00</p>
      </div>
    </div>

    <div class="stavka" :class="{ 'najskuplja': najskupljaStavka === 'Mrkva' }">
      <img :src="slike.Mrkva" alt="Mrkva">
      <div class="info">
        <p><strong>Mrkva</strong></p>
        <p>Cijena: €0.12 | Količina: 12</p>
        <p>Ukupno: €1.44</p>
      </div>
    </div>

    <div class="stavka" :class="{ 'najskuplja': najskupljaStavka === 'Kruh' }">
      <img :src="slike.Kruh" alt="Kruh">
      <div class="info">
        <p><strong>Kruh</strong></p>
        <p>Cijena: €2.00 | Količina: 3</p>
        <p>Ukupno: €6.00</p>
      </div>
    </div>

    
    <div class="stavka" :class="{ 'najskuplja': najskupljaStavka === 'Sir' }">
      <img :src="slike.Sir" alt="Sir">
      <div class="info">
        <p><strong>Sir</strong></p>
        <p>Cijena: €4.48 | Količina: 1</p>
        <p>Ukupno: €4.48</p>
      </div>
    </div>

    
    <p class="ukupna-cijena"><strong>Ukupna cijena:</strong> {{ sveukupnaCijena }} €</p>
  </div>
</template>

