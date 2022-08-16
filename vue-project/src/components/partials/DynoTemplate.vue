<template src="./DynoTemplate.html"></template>

<script lang="ts">
import { defineComponent } from 'vue';
interface DynoTemp{
    color:Boolean,
    img:String,
    mission_name:String,
    details:String,
    read_more:String,
    h31:String,
    h31Data:String,
    h32:String,
    h32Data:String,
    h33:String,
    h33Data:String
}
export default defineComponent({
    props:{
        apiEnd:String
    },
    mounted: function () {
        document.addEventListener("scroll",this.loadMore)
        this.callApi(20,0)
    },
    data: function() {
        return{
            infoProp:[] as infoProp[],
            offSet:0,
            url:''     
        }
    },
    methods:{
        callApi(lmt=10,offset=this.offSet){
            let fetchUrl = `https://api.spacexdata.com/v3/${this.apiEnd}?limit=${lmt}&offset=${offset}`

            if (this.url !== fetchUrl) {
                this.url=fetchUrl;
                fetch(fetchUrl)
                    .then(res => {
                        if (res.status === 200) {
                            return res.json()
                        } else {
                            throw Error(res.statusText);
                        }
                    })
                    .then(res => {
                        if (res.length > 0) {
                            this.infoProp = [...this.infoProp, ...res];
                            this.offSet = this.offSet + lmt
                        }
                    }).catch((err) => {
                        console.warn(err)
                    });
            }
            else{
                return ;
            }
            
        },
        loadMore(){
            let bottomOfWindow = Math.ceil(document.documentElement.scrollTop + window.innerHeight) === Math.round(document.documentElement.offsetHeight);
            //console.log(bottomOfWindow,document.documentElement.scrollTop,window.innerHeight,document.documentElement.offsetHeight/2)
            if(bottomOfWindow){
                this.callApi();
            }
        },
        getData(info){
            let ret:DynoTemp;
            switch (this.apiEnd){
                case "Missions":
                    ret={
                        color:false,
                        img:'',
                        mission_name:"mission_name",
                        details:"description",
                        read_more:"website",
                        h31:"Twitter",
                        h31Data:"twitter",
                        h32:"Wikipedia",
                        h32Data:"wikipedia",
                        h33:"",
                        h33Data:""
                    }
                    return ret;
                case "Capsules":
                    ret={
                        color:info.status==='retired'?true:false,
                        img:'',
                        mission_name:'capsule_id',
                        details:"details",
                        read_more:"",
                        h31:"Status",
                        h31Data:"status",
                        h32:"Type",
                        h32Data:"type",
                        h33:"Landings",
                        h33Data:"landings"
                        }
                    return ret;
                case "Ships":
                    ret={
                        color:info.active===false?true:false,
                        img:'image',
                        mission_name:'ship_name',
                        details:"home_port",
                        read_more:"url",
                        h31:"Year",
                        h31Data:"year_built",
                        h32:"Status",
                        h32Data:"status",
                        h33:"Missions",
                        h33Data:info.missions.length>0?info.missions.length:"No data"
                        }
                    return ret;
                case "Rockets":
                    ret={
                        color:info.active===false?true:false,
                        img:'',
                        mission_name:'rocket_name',
                        details:"description",
                        read_more:"wikipedia",
                        h31:"Country",
                        h31Data:"country",
                        h32:"Engine",
                        h32Data:info.engines.type,
                        h33:"First Flight",
                        h33Data:"first_flight"
                        }
                    return ret;
                default:
                    return
            }
        }
    }
})
</script>