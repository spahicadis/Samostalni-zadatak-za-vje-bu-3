<script setup>
import { ref, reactive, computed } from 'vue'
const proizvodi = reactive([])
const inputNaziv = ref("")
const inputCijena = ref("")


const ukupnoProizvod = (proizvodCijena, proizvodKolicina) => {
  return proizvodCijena * proizvodKolicina
}

const provjeraProzivoda = (naziv) => {
  return proizvodi.findIndex((proizvod) => proizvod.proizvodNaziv === naziv)
}


const dodaj = (naziv, cijena) => {

const provjera = provjeraProzivoda(naziv) //Tu dobijamo index postojeceg, ako ga nema vraca -1 zato je takva usporedba
if(provjera >= 0) { 
  proizvodi[provjera].proizvodKolicina += Number(1)
  inputNaziv.value = ""
  inputCijena.value = ""
  return
}

if(cijena < 0) {
  cijena = 0
  alert("Cijena ne smije biti manja od 0")
}


  proizvodi.push({
    proizvodNaziv: naziv,
    proizvodKolicina: Number(0),
    proizvodCijena: cijena
    
  })
  inputNaziv.value = ""
  inputCijena.value = ""
}

const smanji = (index) => {
  if(proizvodi[index].proizvodKolicina <= 0) {
    return
  }
  proizvodi[index].proizvodKolicina--

}
const povecaj = (index) => {
  proizvodi[index].proizvodKolicina++
}


const obrisi = (index) => {
  proizvodi.splice(index, 1)
}


const ukupnoKosarica = () => {
  let ukupno = 0;
  proizvodi.forEach((item) => {
    ukupno += item.proizvodKolicina * item.proizvodCijena;
  })

  return ukupno
}




</script>


<template>


<div class="min-w-6xl p-6 flex flex-col gap-1.5 rounded-md shadow-md bg-gray-100 border border-gray-200">
  <h2 class="text-3xl font-medium">Košarica</h2>
  <hr class="border-gray-200 border mt-1"/>

  <div class="w-full min-h-20 flex items-center justify-between py-2 px-4">
    <label class="">Naziv proizvoda:</label>
    <input type="text" class="border rounded-sm p-1 max-h-[32px]" v-model="inputNaziv"/>
    <label class="">Cijena proizvoda:</label>
    <input type="number" class="border rounded-sm max-h-[32px] p-1" v-model="inputCijena"/>
    <button :disabled="!inputNaziv" :class="inputNaziv ? 'cursor-pointer' : 'cursor-not-allowed opacity-50'"  class="py-1 px-2 rounded-md bg-green-400 shadow-md" @click="dodaj(inputNaziv, inputCijena)">Dodaj artikl</button>
  </div>
  <hr class="border-gray-200 border mt-1"/>

  <table class="table-auto mt-3 w-full bg-white rounded-md shadow-lg">
 <thead class="h-7 w-full">
  <tr v-if="proizvodi.length === 0">
    <th>Kosarica je prazna</th>
  </tr>
    <tr v-else>
      <th>Naziv</th>
      <th >Količina</th>
      <th >Cijena</th>
      <th>Ukupno</th>
      <th>Akcije</th>
    </tr>
  </thead>
  <tbody>
    <tr 
    v-for="(proizvod, index ) in proizvodi"
    :key="index"
    >
    <td class="text-center py-2">
      {{ proizvod.proizvodNaziv }}
    </td>
    <td class="text-center">
      <button @click="smanji(index)" class="mr-6 cursor-pointer">-</button> 
      <input v-model="proizvod.proizvodKolicina" type="text" class="w-12 border rounded-md text-center"/> 
      <button @click="povecaj(index)" class="ml-6 cursor-pointer">+</button>
    </td>  
    <td class="text-center">
      {{ proizvod.proizvodCijena }}
    </td>
    <td class="text-center">
      {{ ukupnoProizvod(proizvod.proizvodCijena, proizvod.proizvodKolicina) }}
    </td>
    <td  @click="obrisi(index)" class="py-2 text-center text-red-600">
     Ukloni
    </td>
    </tr>
  </tbody>
  </table>

<hr class="border-gray-200 border mt-5"/>
<span>Ukupno: {{ ukupnoKosarica() }}</span>

</div>






</template>