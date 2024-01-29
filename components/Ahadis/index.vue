<template>
   
    <div  class="wallpaper">
        <div class="w-full flex flex-wrap justify-center">
            <div dir="rtl" class="w-full">
                <div class="w-[90%] md:w-[75%] flex justify-end mt-6">
                    <nuxt-link to=".."><span class="text-white rounded-lg bg-cyan-500 p-2 text-center">بازگشت</span></nuxt-link>
                </div>
           </div>
            <div class="w-full flex justify-center">

                <div dir="rtl" class="flex flex-wrap bg-[rgb(0,100,140,0.8)] text-white w-[80%] md:w-[50%] mt-20 px-2 py-4 rounded">
                    <p class="w-full font-bold text-[14px] md:text-[16px]">{{ hadis?.text }}</p>
                    <span class="w-full flex justify-end mt-2 text-[12px]">{{  hadis?.person ? '(' + hadis?.person + ')' : ' '  }}</span>
                </div>
            </div>
            <div class="w-[80%] md:w-[50%] p-2">
                <button @click="refreshHadis" class="btn btn-sm btn-active btn-warning text-white text-[14px]">بعدی</button>
            </div>
        </div>
    </div>

</template>

<script setup>
import axios from "axios"


const hadis = ref(null)
onMounted(()=> {

    axios.get('https://api.keybit.ir/hadis/')
    .then((res)=>{
        hadis.value = res.data.result
    })
})

function refreshHadis(){
    refresh()
}

function refresh(){
    axios.get('https://api.keybit.ir/hadis/')
    .then((res)=>{
        hadis.value = res.data.result
    })
}
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