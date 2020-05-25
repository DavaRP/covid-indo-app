<template>
<div>
  <v-sheet light elevation="3" style="border-radius : 0px 0px 30px 30px">
    <v-container>
      <v-row justify="center">
        <v-col cols="11">
          <div class="mb-5">
            <v-row>
              <v-col cols="5">
                <div class="d-flex flex-row">
                  <h2 class="display-3 mt-2 font-weight-bold"><i class="mdi mdi-chart-areaspline"></i></h2>
                  <div style="width : 100% !important" class="mt-2">
                    <span style="font-size : 7pt !important">Total Kasus</span>
                    <p style="font-size : 16pt !important">{{convertNumber(totalCase.total)}} </p>
                  </div>
                </div>
              </v-col>
              <v-col cols="7">
                <div class="d-flex flex-row">
                  <div style="width : 100% !important; border-right : 1px solid black; height :55px" class="text-center">
                    <span style="font-size : 7pt !important">Sembuh</span>
                    <p style="font-size : 12pt !important">{{convertNumber(totalCase.sembuh)}} </p>
                  </div>
                  <div  class="text-center" style="width : 100% !important; border-right : 1px solid black; height :55px" >
                    <span style="font-size : 7pt !important">Perawatan</span>
                    <p style="font-size : 12pt !important">{{convertNumber(totalCase.perawatan)}} </p>
                  </div>
                  <div  class="text-center" style="width : 100% !important" >
                    <span style="font-size : 7pt !important">Meninggal</span>
                    <p style="font-size : 12pt !important">{{convertNumber(totalCase.meninggal)}} </p>
                  </div>
                </div>
                <v-btn :to="'/statistic'" x-small block elevation="0"><v-icon size="14" class="mr-2">mdi-eye</v-icon> Lihat Statistik</v-btn>
              </v-col>  
            </v-row>
          </div>
          <v-autocomplete
            v-model="search"
            :items="province"
            dense
            outlined
            :placeholder="search ? null : 'Pilih Provinsi'"
            append-outer-icon="mdi-magnify"
            clearable
            @click:append-outer="searchProvince(search)"
            @keypress.enter="searchProvince(search)"
            background-color="white"
            label="Data Provinsi COVID "
          ></v-autocomplete>
        </v-col>
      </v-row>
    </v-container>
  </v-sheet>
  <v-row justify="center" class="mt-3">
    <v-col cols="11">
      <v-icon>mdi-cloud-refresh</v-icon> <small class="ml-2 font-weight-light">Last Update</small>
      <div>{{(new Date().getDate()) + ', ' + convertMonth(new Date().getMonth() + 1) + ' ' + new Date().getFullYear()}}</div>
    </v-col>
  </v-row>
  <v-row justify="center" >
    <v-col cols="11">
      <div class="d-flex justify-space-between">
        <div>
          <v-icon>mdi-information</v-icon> <small class="ml-1">Informasi</small>
        </div>
        <div>
          <span class="mr-1 font-weight-light">{{search ? search : 'Pilih Provinsi'}}</span>
          <v-icon>mdi-crosshairs</v-icon> 
        </div>
      </div>
    </v-col>
  </v-row>
  <v-row justify="center" style="height : 50vh !important" id="content">
    <v-col cols="11">
      <v-row>
        <v-col cols="6">
            <v-card height="120" width="100%" light elevation="7">
              <v-card-text>
                <small class="grey--text">Positif</small>
                <h2 class="display-1 font-weight-bold">{{convertNumber(data.kasusPosi)}}</h2>
                <h3 class="float-right font-weight-light">Orang</h3>
              </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="6">
          
            <v-card height="120" width="100%" light elevation="7">
              <v-card-text>
                <small class="grey--text">Meninggal</small>
                <h2 class="display-1 font-weight-bold">{{convertNumber(data.kasusMeni)}}</h2>
                <h3 class="float-right font-weight-light">Orang</h3>
              </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="6">
            <v-card height="120" width="100%" light elevation="7">
              <v-card-text>
                <small class="grey--text">Sembuh</small>
                <h2 class="display-1 font-weight-bold">{{convertNumber(data.kasusSemb)}}</h2>
                <h3 class="float-right font-weight-light">Orang</h3>
              </v-card-text>
            </v-card>
        </v-col>
        <!-- <v-col cols="12" class="text-center mt-10">
          <small>Dava Rizqi Pradipta <v-icon>mdi-copyright</v-icon> 2020</small>
        </v-col> -->
      </v-row>
    </v-col>
  </v-row>
</div>
</template>

<script>
import numeral from 'numeral'
import axios from 'axios'
export default {
  name: 'Home',
  data () {
    return { 
      province: [] ,
      covid: [] ,
      search : null,
      data : {
        kasusPosi : 0,
        kasusSemb : 0,
        kasusMeni : 0,
      },
      totalCase : {
        total : null,
        meninggal : null,
        perawatan : null,
        sembuh : null
      }
    }
  },
  mounted () {
    if(localStorage.getItem('data')){
      this.data = JSON.parse(localStorage.getItem('data'))
      this.search = localStorage.getItem('search')
    }
    this.getCovidData()
  },
  methods : {
    searchProvince(search){
      function searchData (element){
          return element.provinsi === search
      }
      var data = this.covid.findIndex(searchData)
      this.data = this.covid[data]
      localStorage.setItem( 'data' ,JSON.stringify(this.data))
      localStorage.setItem( 'search' , search)
    },
    convertNumber(number){
      return numeral(number).format('0,0')
    },
    convertMonth(month){
      if(month == 0){
        return 'January'
      }else if(month == 1){
        return 'February'
      }else if(month == 2){
        return 'March'
      }else if(month == 3){
        return 'April'
      }else if(month == 4){
        return 'May'
      }else if(month == 5){
        return 'June'
      }else if(month == 6){
        return 'July'
      }else if(month == 7){
        return 'August'
      }else if(month == 8){
        return 'September'
      }else if(month == 9){
        return 'October'
      }else if(month == 10){
        return 'November'
      }else if(month == 11){
        return 'December'
      }
    },
    async getCovidData(){
      await axios.get('https://indonesia-covid-19.mathdro.id/api/provinsi').then(async res =>  {
        this.covid = await res.data.data
        this.covid.forEach(element => {
          this.province.push(element.provinsi)
        });
      })
      await axios.get('https://indonesia-covid-19.mathdro.id/api/').then(async res => {
        this.totalCase.total = res.data.jumlahKasus
        this.totalCase.sembuh = res.data.sembuh
        this.totalCase.perawatan = res.data.perawatan
        this.totalCase.meninggal = res.data.meninggal
      })
    },
  }
}
</script>

<style scoped>
#content {
  background : url('../assets/map.png') !important
  /* background : black !important */
}
</style>
