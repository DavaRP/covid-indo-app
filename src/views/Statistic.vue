<template>
  <v-sheet elevation="0" style="border-radius : 0px " height="100%" light>
    <div>
        <v-container>
            <v-row>
                <v-col cols="12">
                    <h2 class="display-2 mb-4">{{(new Date().getDate()) + ', ' + convertMonth(new Date().getMonth() + 1) + ' ' + new Date().getFullYear()}}</h2>
                <small class="grey--text">Kasus Terkonfirmasi</small>
                <div class="d-flex flex-row">
                    <h2 class="display-3 mt-2 font-weight-bold"><i class="mdi mdi-chart-areaspline"></i></h2>
                    <div style="width : 100% !important" class="mt-2 ml-3">
                        <span style="font-size : 7pt !important">Jumlah</span>
                        <p style="font-size : 16pt !important">{{convertNumber(confirmed.data[(confirmed.data).length - 1].Cases)}} </p>
                    </div>
                </div>
                </v-col>
            </v-row>
        </v-container>
        <v-sparkline
            :value="confirmed.Cases"
            color="rgba(100, 100, 100, .7)"
            height="200"
            padding="16"
            stroke-linecap="round"
            smooth
        >
        </v-sparkline>
        <v-row>
            <v-col>
                <div class="d-flex flex-row">
                    <div style="width : 100% !important; border-right : 1px solid black; height :55px" class="text-center">
                    <span style="font-size : 7pt !important">Dari Tanggal </span>
                    <p style="font-size : 12pt !important">{{convertDate(confirmed.data[0].Date)}} </p>
                    </div>
                    <div  class="text-center" style="width : 100% !important;  height :55px" >
                    <span style="font-size : 7pt !important">Hingga Tanggal</span>
                    <p style="font-size : 12pt !important">{{convertDate(confirmed.data[(confirmed.data).length - 1].Date)}} </p>
                    </div>
                </div>
            </v-col>
        </v-row>
    </div>
    <div>
        <v-container>
            <v-row>
                <v-col cols="12">
                <small class="grey--text">Kasus Meninggal</small>
                <div class="d-flex flex-row">
                    <h2 class="display-3 mt-2 font-weight-bold"><i class="mdi red--text mdi-skull-outline"></i></h2>
                    <div style="width : 100% !important" class="mt-2 ml-3">
                        <span style="font-size : 7pt !important">Jumlah</span>
                        <p style="font-size : 16pt !important">{{convertNumber(deaths.data[(deaths.data).length - 1].Cases)}} </p>
                    </div>
                </div>
                </v-col>
            </v-row>
        </v-container>
        <v-sparkline
            :value="deaths.Cases"
            color="rgba(255, 100, 100, .7)"
            height="200"
            padding="16"
            stroke-linecap="round"
            smooth
        >
        </v-sparkline>
        <v-row>
            <v-col>
                <div class="d-flex flex-row">
                    <div style="width : 100% !important; border-right : 1px solid black; height :55px" class="text-center">
                    <span style="font-size : 7pt !important">Dari Tanggal </span>
                    <p style="font-size : 12pt !important">{{convertDate(deaths.data[0].Date)}} </p>
                    </div>
                    <div  class="text-center" style="width : 100% !important;  height :55px" >
                    <span style="font-size : 7pt !important">Hingga Tanggal</span>
                    <p style="font-size : 12pt !important">{{convertDate(deaths.data[(deaths.data).length - 1].Date)}} </p>
                    </div>
                </div>
            </v-col>
        </v-row>
    </div>
    <div>
        <v-container>
            <v-row>
                <v-col cols="12">
                <small class="grey--text">Kasus Sembuh</small>
                <div class="d-flex flex-row">
                    <h2 class="display-3 mt-2 font-weight-bold"><i class="mdi mdi-shield-check-outline green--text"></i></h2>
                    <div style="width : 100% !important" class="mt-2 ml-3">
                        <span style="font-size : 7pt !important">Jumlah</span>
                        <p style="font-size : 16pt !important">{{convertNumber(recovered.data[(recovered.data).length - 1].Cases)}} </p>
                    </div>
                </div>
                </v-col>
            </v-row>
        </v-container>
        <v-sparkline
            :value="recovered.Cases"
            color="rgba(100, 255, 100, .7)"
            height="200"
            padding="16"
            stroke-linecap="round"
            smooth
        >
        </v-sparkline>
        <v-row>
            <v-col>
                <div class="d-flex flex-row">
                    <div style="width : 100% !important; border-right : 1px solid black; height :55px" class="text-center">
                    <span style="font-size : 7pt !important">Dari Tanggal </span>
                    <p style="font-size : 12pt !important">{{convertDate(recovered.data[0].Date)}} </p>
                    </div>
                    <div  class="text-center" style="width : 100% !important;  height :55px" >
                    <span style="font-size : 7pt !important">Hingga Tanggal</span>
                    <p style="font-size : 12pt !important">{{convertDate(recovered.data[(recovered.data).length - 1].Date)}} </p>
                    </div>
                </div>
            </v-col>
        </v-row>
    </div>
  </v-sheet>
</template>

<script>
import axios from 'axios'
import numeral from 'numeral'
export default {
    data () {
        return {
            covid: [] ,
            confirmed : {
                data : [{
                    Cases : 0,
                    Date : '-'
                }],
                Cases : [],
            },
            deaths : {
                data : [{
                    Cases : 0,
                    Date : '-'
                }],
                Cases : [],
            },
            recovered : {
                data : [{
                    Cases : 0,
                    Date : '-'
                }],
                Cases : [],
            },
        }
    },
    mounted () {
        this.getCovidData()
    },
    methods : {
    convertDate(date){
        return (new Date(date).getDate()) + ', ' + this.convertMonth(new Date(date).getMonth() + 1) + ' ' + new Date(date).getFullYear()
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
      await axios.get('https://api.covid19api.com/dayone/country/Indonesia/status/confirmed/live').then(async res => {
          this.confirmed.data = res.data
            var no = 1
            var next 
            var prev 
            for (let i = 0; i < this.confirmed.data.length; i++) {
                const prev = this.confirmed.data[i].Cases; // 0
                if(i != this.confirmed.data.length - 1)
                {
                    const next = this.confirmed.data[i + 1].Cases; // 0
                    var selisih = next - prev
                    this.confirmed.Cases.push(selisih)
                }
            }
      })
      await axios.get('https://api.covid19api.com/dayone/country/Indonesia/status/recovered/live').then(async res => {
          this.recovered.data = res.data
            var no = 1
            var next 
            var prev 
            for (let i = 0; i < this.recovered.data.length; i++) {
                const prev = this.recovered.data[i].Cases; // 0
                if(i != this.recovered.data.length - 1)
                {
                    const next = this.recovered.data[i + 1].Cases; // 0
                    var selisih = next - prev
                    this.recovered.Cases.push(selisih)
                }
            }
      })
      await axios.get('https://api.covid19api.com/dayone/country/Indonesia/status/deaths/live').then(async res => {
          this.deaths.data = await res.data
            var no = 1
            var next 
            var prev 
            for (let i = 0; i < this.deaths.data.length; i++) {
                const prev = this.deaths.data[i].Cases; // 0
                if(i != this.deaths.data.length - 1)
                {
                    const next = this.deaths.data[i + 1].Cases; // 0
                    var selisih = next - prev
                    this.deaths.Cases.push(selisih)
                }
            }
        })
      

    },
    }
}
</script>

<style>

</style>