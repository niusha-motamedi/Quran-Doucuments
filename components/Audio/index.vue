<template>
    <div dir="rtl" class="wallpaper">
      <div class="w-full flex flex-wrap justify-center">
        <div class="w-full">
                <div class="w-[90%] md:w-[70%] flex justify-end mt-6">
                    <nuxt-link to=".."><span class="text-white rounded-lg bg-cyan-500 p-2 text-center">بازگشت</span></nuxt-link>
                </div>
           </div>
        <label class="form-control w-full max-w-xs mt-8">
          <div class="label">
          </div>
          <select @change="surahSelected" v-model="selectSurah" class="select select-bordered select-info">
            <option disabled selected>انتخاب سوره</option>
            <option v-for="(item, index) in surahsList" :key="index">{{ item }}</option>
          </select>
        </label>
        
        <div class="w-full flex justify-center mt-8">
          <audio ref="audioPlayer" controls @ended="playNext">
            <source :src="currentAudio" type="audio/mp3" />
          </audio>
        </div>
        
        <div v-if="surah !== null" class="w-[90%] md:w-[50%] flex flex-wrap border-2 border-white text-white bg-[rgb(50,80,150,0.8)] mt-4 rounded-md p-2 leading-8 md:leading-10">
          <div v-if="surah[0].name !== 'سُورَةُ ٱلْفَاتِحَةِ' && surah[0].name !== 'سُورَةُ التَّوۡبَةِ'" class="w-full flex border-b border-black py-2">
            
            <p class="w-[100%] flex justify-center opacity-100">"بِسْمِ ٱللَّهِ ٱلرَّحْمَٰنِ ٱلرَّحِيمِ"</p>
          </div>
          <div  v-for="(item, index) in surah[0].ayahs" :key="index" :id="index" class="w-full flex border-b border-black py-2">
            <span class="font-bold w-[10%] flex justify-start md:justify-center">{{ item.numberInSurah }} .</span>
            <p class="w-[90%] flex justify-start">{{ item.text }}</p>
          </div>
        </div>
       </div>
      </div>
    </template>
  
  <script setup>
  import { ref, onMounted, watch } from 'vue';
  import axios from 'axios';
  
  const myData = ref([]);
  const surahsList = ref([]);
  const audioPlayer = ref(null);
  const audioUrls = ref([]);
  const currentAudioIndex = ref(0);
  
  onMounted(async () => {
    try {
      const response = await axios.get('https://api.alquran.cloud/v1/quran/ar.alafasy');
      myData.value = response.data.data.surahs;
  
      for (let i = 0; i < myData.value.length; i++) {
        surahsList.value.push(myData.value[i].name);
      }
  
    } catch (error) {
      console.log('Error fetching data:', error);
    }
  });
  
  const surah = ref(null);
  const selectSurah = ref('انتخاب سوره');
  
  const surahSelected = () => {

    surah.value = myData.value.filter((val) => val.name === selectSurah.value);

    if(document.getElementById(currentAudioIndex.value) !== null){

     document.getElementById(currentAudioIndex.value - 1).classList.remove('text-red-500')
 }
 
    audioUrls.value = [];
  
    if(surah.value[0].name !== 'سُورَةُ ٱلْفَاتِحَةِ' && surah.value[0].name !== 'سُورَةُ التَّوۡبَةِ'){
        audioUrls.value.push({
    
     audio: "https://cdn.islamic.network/quran/audio/64/ar.alafasy/1.mp3"
   
  });
    }
    for (let i = 0; i < surah.value[0].ayahs.length; i++) {
    //   let ayahText = surah.value[0].ayahs[i].text;
  
      // Remove "بِسْمِ ٱللَّهِ ٱلرَّحْمَٰنِ" only from the first verse
      if (i === 0 && surah.value[0].ayahs[i].text.startsWith("بِسْمِ ٱللَّهِ ٱلرَّحْمَٰنِ ٱلرَّحِيمِ")) {
        surah.value[0].ayahs[i].text = surah.value[0].ayahs[i].text.substring("بِسْمِ ٱللَّهِ ٱلرَّحْمَٰنِ ٱلرَّحِيمِ".length).trim();
      }
  
      audioUrls.value.push({
    
        audio: surah.value[0].ayahs[i].audio,
       
      });
    }

    if (audioUrls.value.length > 0) {
        
      audioPlayer.value.src = audioUrls.value[0].audio;
      audioPlayer.value.play();
    }
  };
  
  const playNext = () => {
    if (currentAudioIndex.value < audioUrls.value.length - 1) {
      currentAudioIndex.value++;
      audioPlayer.value.src = audioUrls.value[currentAudioIndex.value].audio;
      document.getElementById( surah.value[0].name !== 'سُورَةُ ٱلْفَاتِحَةِ' ? currentAudioIndex.value - 1 : currentAudioIndex.value).classList.add('text-red-500')
      if(currentAudioIndex.value > 1){
        document.getElementById(surah.value[0].name !== 'سُورَةُ ٱلْفَاتِحَةِ' ?  currentAudioIndex.value - 2 : currentAudioIndex.value - 1 ).classList.remove('text-red-500')
      }
       (currentAudioIndex.value);
      audioPlayer.value.play();
    }
  };
  
  watch(audioUrls, () => {
    currentAudioIndex.value = 0;
  });
  
  watch(currentAudioIndex, () => {
    audioPlayer.value.src = audioUrls.value[currentAudioIndex.value].audio;
    audioPlayer.value.play();
  });
  </script>
  
  <style scoped>
 
.wallpaper{
    background-image: url('~/assets/img/picture4.jpeg');
    background-size: cover;
    background-repeat: no-repeat;
    background-attachment: fixed;
    min-height: 100vh;
  }

  </style>
  