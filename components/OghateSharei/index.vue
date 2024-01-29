
import type { Axios } from 'axios';

import type { Axios } from 'axios';

import type { Axios } from 'axios';
<template>
   
    <div dir="rtl" class="wallpaper">
        <div class="w-full flex flex-wrap justify-center">
           <div class="w-full">
                <div class="w-[90%] md:w-[70%] flex justify-end mt-6">
                    <nuxt-link to=".."><span class="text-white rounded-lg bg-cyan-500 p-2 text-center">بازگشت</span></nuxt-link>
                </div>
           </div>
            <div class="w-[90%] md:w-[40%] lg:w-[20%] md flex flex-wrap justify-center mt-4">
                <label class="form-control w-full max-w-xs">
                    <div class="label">
                        <span class="label-text-alt text-white md:text-[16px]">شهر خود را وارد نمایید</span>
                    </div>
                    <input v-model="myCity" type="text" placeholder="مثال : تهران" class="input input-bordered w-full max-w-xs" />
                </label>
                <div class="w-full flex justify-center  mt-1">
                    <button @click="getOwghat" class="btn btn-success text-white btn-sm">دریافت</button>
                </div>
            </div>
            <div class="w-full flex justify-center">

                <div v-if="trueData" class="w-[90%] md:w-[50%] lg:w-[40%] text-white rounded-sm bg-[rgb(0,100,140,0.8)] mt-4">
                    <div class="w-full justify-start p-2 mt-1">شهر : &nbsp <span>{{myData?.city}}</span></div>
                    <div class="w-full justify-start p-2 mt-1">تاریخ : &nbsp <span>{{ myData?.day }} / {{myData?.month}}</span></div>
                    <div class="w-full justify-start p-2 mt-1">طلوع آفتاب : &nbsp <span>{{myData?.toloe_aftab}}</span></div>
                    <div class="w-full justify-start p-2 mt-1">غروب آفتاب : &nbsp <span>{{myData?.ghorob_aftab}}</span></div>
                    <div class="w-full justify-start p-2 mt-1">اذان صبح : &nbsp <span>{{myData?.azan_sobh}}</span></div>
                    <div class="w-full justify-start p-2 mt-1">اذان ظهر : &nbsp <span>{{myData?.azan_zohre}}</span></div>
                    <div class="w-full justify-start p-2 mt-1">اذان مغرب : &nbsp <span>{{myData?.azan_maghreb}}</span></div>
                    <div class="w-full justify-start p-2 mt-1">نیمه شب شرعی : &nbsp <span>{{myData?.nime_shabe_sharie}}</span></div>
                    <!-- <div class="w-full justify-start p-2 mt-1">تاریخ : &nbsp <span>{{myData.azan_sobh}}</span></div> -->
                    
                </div>
                
                <div v-if="error" class="w-[90%] md:w-[50%] lg:w-[40%] h-[100px] flex justify-center items-center md:text-[18px] text-red-500 text-center rounded-sm bg-[rgb(0,100,140,0.8)] mt-4">
                    شهر وارد شده وجود ندارد
                </div>
            </div>

       
        </div>
    </div>

</template>

<script setup>
import axios from "axios"


const myCity = ref(null)
const myData = ref(null)
const error = ref(false)
const trueData = ref(false)
function getOwghat(){
    axios.get(`https://one-api.ir/owghat/?token=491873:65b7fde6408f8&city=${myCity.value}&en_num=${false}`)
    .then((res)=>{
        if(res.data.status === 200){
            trueData.value = true
            error.value = false
            myData.value = res.data.result
        }else if(res.data.status === 404){
            trueData.value = false
            error.value = true
        }


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