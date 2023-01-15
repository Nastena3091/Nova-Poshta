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
                searchedWarehouse:''
            }
            
        },	 
        mounted: function(){
            axios.post("https://api.novaposhta.ua/v2.0/json/",{
                "apiKey": "22e5e73d92a0e5d2768eda097e599888",
                "modelName": "Address",
                "calledMethod": "getCities",
                "methodProperties": {
                }
            }).then(res => {
                    // console.log(res.data.data);
                    this.cities = res.data.data;
                    // this.cities.forEach((element)=>{
                    //     console.log(element.Description)
                    // })
                })
        },
        methods: {
            selectWarehouses(){
                console.log("test")
                axios.post("https://api.novaposhta.ua/v2.0/json/",{
                    "apiKey": "22e5e73d92a0e5d2768eda097e599888",
                    "modelName": "Address",
                    "calledMethod": "getWarehouses",
                    "methodProperties": {
                        "CityName": this.city,
                        "TypeOfWarehouseRef":''
                    }
                }).then(res => {
                    console.log(res.data.data);
                    this.warehouses = res.data.data;
                    this.warehouses.forEach((element)=>{
                        console.log(element.Description)
                    })
                })
            },
            searchCity(){
                if(this.searchedCity){
                    axios.post("https://api.novaposhta.ua/v2.0/json/",{
                        "apiKey": "22e5e73d92a0e5d2768eda097e599888",
                        "modelName": "Address",
                        "calledMethod": "getWarehouses",
                        "methodProperties": {
                            "CityName": this.searchedCity
                        }
                    }).then(res => {
                        console.log(res.data.data);
                        this.searchedWarehouses = res.data.data;
                        this.searchedWarehouses.forEach((element)=>{
                            console.log(element.Description)
                        })
                    })
                }
            }
        },
    }
</script>

<template>
    <div class="example">
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
    </div>
    <div class="example">
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
    </div>
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
