<script>
  import axios from 'axios';

  export default {
        data(){
            return {
                cities:[],
                city:'',
                warehouses:[],
                warehouse:'',
                searchedCity:'',
                searchedWarehouses:[],
                searchedWarehouse:'',
                area:'',
                areas:[],
                findArea:[]
            }
            
        },	 
        mounted: function(){
            axios.post("https://api.novaposhta.ua/v2.0/json/",{
                "apiKey": "22e5e73d92a0e5d2768eda097e599888",
                "modelName": "Address",
                "calledMethod": "getAreas",
                "methodProperties": {}
                }).then(res => {
                    console.log(res.data.data);
                    this.areas = res.data.data;
                })
        },
        methods: {
            selectWarehouses(){
                axios.post("https://api.novaposhta.ua/v2.0/json/",{
                    "apiKey": "22e5e73d92a0e5d2768eda097e599888",
                    "modelName": "Address",
                    "calledMethod": "getWarehouses",
                    "methodProperties": {
                        "CityName": this.city
                    }
                }).then(res => {
                    console.log(res.data.data);
                    this.warehouses = res.data.data;
                    this.warehouses.forEach((element)=>{
                        console.log(element.Description)
                    })
                })
            },
            // searchCity(){
            //     if(this.searchedCity){
            //         axios.post("https://api.novaposhta.ua/v2.0/json/",{
            //             "apiKey": "22e5e73d92a0e5d2768eda097e599888",
            //             "modelName": "Address",
            //             "calledMethod": "getWarehouses",
            //             "methodProperties": {
            //                 "CityName": this.searchedCity
            //             }
            //         }).then(res => {
            //             console.log(res.data.data);
            //             this.searchedWarehouses = res.data.data;
            //             this.searchedWarehouses.forEach((element)=>{
            //                 console.log(element.Description)
            //             })
            //         })
            //     }
            // },
            selectCity(){
                // console.log(this.areas)
                // this.findArea=this.areas.filter(element => {
                //     element.Description==this.area
                // });
                // console.log(this.findArea)
                axios.post("https://api.novaposhta.ua/v2.0/json/",{
                "apiKey": "22e5e73d92a0e5d2768eda097e599888",
                "modelName": "Address",
                "calledMethod": "getCities",
                "methodProperties": {
                    "AreaRef":this.area
                }
            }).then(res => {
                    // console.log(res.data.data);
                    this.cities = res.data.data;
                    // this.cities.forEach((element)=>{
                    //     console.log(element.Description)
                    // })
                })
            }
        },
    }
</script>

<template>
    <div class="example">
        <div class="block">
            <p>Область:</p>
            <select id="city" v-model="area" @change="selectCity">
                <option value="" disabled selected>Область</option>
                <option v-for="a in areas" :key="a.Ref" :value="a.Ref">{{a.Description}}</option>
            </select>
        </div>
        <div class="block">
            <p>Населений пункт:</p>
            <select id="city" v-model="city" @change="selectWarehouses">
                <option value="" disabled selected>Населений пункт</option>
                <option v-for="c in cities" :key="c.CityID" >{{c.Description}}</option>
            </select>
        </div>
        <div class="block">
            <p>Відділення:</p>
            <select id="city" v-model="warehouse">
                <option disabled selected value="">Відділення</option>
                <option v-for="w in warehouses" :key="w.Ref">{{w.Description}}</option>
            </select>
        </div>
        <div class="block">
            <p>Ви обрали: <br>
               Населений пункт: {{city}} <br>
               Відділення: {{ warehouse }}</p>
        </div>
    </div>
    <!-- <div class="example">
        <div class="block">
            <p>Населений пункт:</p>
            <input type="text" v-model.lazy="searchedCity" @keydown.enter="searchCity" placeholder="Населений пункт">
        </div>
        <div class="block">
            <p>Відділення:</p>
            <select id="city" v-model="searchedWarehouse">
                <option disabled selected value="">Відділення</option>
                <option v-for="s in searchedWarehouses" :key="s.Ref">{{s.Description}}</option>
            </select>
        </div>
        <div class="block">
            <p>Ви обрали: <br>
            Населений пункт: {{searchedCity}} <br>
            Відділення: {{ searchedWarehouse }}</p>
        </div>
    </div> -->
</template>

<style scoped>
.block{
    margin: 20px 10px
}
.block select {
    padding: 10px;
    width: 400px;
}
.block input{
    width: 376px;
    padding: 10px;
}
.example{
    margin:50px 0 ;
}
</style>
