<template>
  <div class="home">
<!-- <p>Benim adim {{ad}} ve yasim {{yas}} </p> Ornek 1 -->
<p ref="refs">Benim adim {{name}} ve yasim {{age}} </p> <!--Ornek 2-->
<button @click="clickIT">Tıkla</button>
<button @click="age++">Increase Age</button>
<input type="text" v-model="name">

<br><br>
<span>ARA</span> <input type="text" v-model="search">
<p>search term - {{search}} </p>
<div v-for="name2 in matchingNames" :key="name2"> {{name2}} </div>
  </div>
</template>

<script>
import { computed, ref, watch, watchEffect } from 'vue'
// @ is an alias to /src

export default {
  name: 'Home',

setup(){


const refs=ref(null);
console.log(refs,refs.value);

// let name = 'Mert'
// let age = 26

const name = ref('Mert');
const age = ref(26);
const search = ref('')
const names = ref(['Mert','Ali','Sefkan','Veli'])

const matchingNames = computed(()=>{
  return names.value.filter((name2)=>name2.includes(search.value))
})

const clickIT = ()=>
{
  console.log('Clicked'); 
  console.log(refs,refs.value);
  name.value = 'Sefkan';
  age.value = '23';
  }


const stopWatch = watch(search,()=>{
  console.log('watch function ran')
})

const stopEffect = watchEffect(()=>{
  console.log('watchEffect function ran');
  search.value;
})

return{name,age,clickIT,refs,matchingNames,search,stopWatch,stopEffect}

}

  }


/*
Setup() component ayaga kalktigi zaman butun lifecycle uyelerinden (created,mounted vb.) once calisir.

Setup içerisinde tanımladıgımız degiskenler,fonksiyonlar vb. template icerisinde kullanmak istediklerimizi return ile dondurebiliyoruz.
"return {ad:name,yas:age}" örnekte goruldugu uzere "ad" burada "name" degisken degerini tutan ve template icerisinde veriyi gostermemize
yarayacak olan propertydir. Direkt olarak degiskenleri de "return{name,age}" olarak dondurup template icerisine basabiliriz.

 */

/* 
** ==> REF

 Composition API da ref kullanımı Options API göre biraz daha farklı olarak tanımlanmakta.Options API da refi tanımlarken kullandığımız
 'this.$refs' yazımını composition API da " const 'degisken adi' = ref ('deger') " olarak tanımlıyoruz.

Composition API da ref() cagırdıgımızda "import { ref } from 'vue'" script tagleri arasına eklemeliyiz.

 Options API da kullandığımız 'this' keywordu componentin kendisini isaret ederken Composition API da bu undefineddır.

Composition API da ref ile ilgili dikkat edilmesi gereken husus ref degerini tutacak degiskenin return den once yazılmasıdır. Bunun sebebi ise
ref, component ilk ayaga kalktıgında tanımladıgımız degeri neyse DOM da onu tutar. Ama component ayaga kalktıgında ref propertysini atadıgımız
deger neyse o deger dondurulur.

ref propertysini genellikle Composition API icerisinde tanimladigimiz degiskenleri de Options API daki gibi reactive yapmak icin kullaniriz.
Bunun icin "const 'degisken adi' = ref('deger')" setup hook icerisinde tanimlariz. Bu degiskeni setup hooktan return{degisken} olarak
dondugumuz de bu degisken template icerisinde bulunuyorsa ilk tanımladıgımız degeri neyse onu alacaktır. Daha sonra bu degeri component
ayaktayken dinamik olarak degistirebilmek icin 'degiskenadi.value = "yeni deger" ' atamasını yaparız. Burada dikkat edilmesi gereken husus
degiskenadi.value olarak setup hook icerisinde eristigimiz ref degerine template icerisinde sadece 'degiskenadi'ni vererek erisebiliriz.
Template icerisinde 'degiskenadi.value' kullanmamıza gerek yoktur.

Refs vs Reactive => Reactive tanımlamasıda aynı ref gibi yapılmaktadır. Aralarındaki farklar ise;

* Reactive ile tanımladıgımız degiskenlerde 'degiskenadi.value' kullanmamıza ihtiyac kalmadan direkt olarak 'degiskendi' ile tutulan degere
erişebiliriz.

* Reactive icerisinde tanımladıgımız primitive tipleri reactive olarak kullanamayız yani template icerisinde ilk degerini degistiremeyiz.
Örneğin; ref('Mert') degeri bir stringdir ve tanımladıktan sonra template icerisinde degeri degistirilebilir.
reactive('Mert') degeri de bir stringdir ama tanımlandıktan sonra template icerisinde degistirilemez.

 */

/**
 * Computed values => Composition api da computed metodunu kullanabilmek icin öncelikle "import { computed } from 'vue'" script tagleri
 * arasına eklemeliyiz. 
 * 
 * Computed values tanımlaması => const 'degisken adi' = computed('deger') seklinde tanımlarız. Computed valuesa, complex verilerin dondurulmesinde
 * örnegin filtreleme gibi kullanmalıyız. 
 */

/*
* watch & watchEffect => Bir olay tetiklendiginde bu olayı takip edip tetiklendigi durumda calısmasını istedigimiz isleri belirttigimiz yapılardır.
Aralarındaki fark ise;
 * watch, takip edilen olay tetiklendiginde fonksiyonu calistirirken,
 * watchEffect, setup hook ilk calismaya basladigi anda bir kere calisir eger icerisinde takip edilecek bir olay varsa bu olay her tetiklendiginde
   tekrardan calisir.

   Watch işlemini durdurmak istedigimiz de ise metotları tekrardan tetiklememiz/çağırmamız yeterli olacaktır.
*/

</script>




