<template src="./SpaceCapsules.html"></template>
<style scoped src="./SpaceCapsules.css"></style>
<script>
export default {

    mounted: function () {
        document.addEventListener("scroll",this.callCapsules)
        this.addCapsules(20,0)
    },
    data: function() {
        return{
            capsules: [],
            offSet:0,
            url:null      
        }
    },
    methods:{
        addCapsules(lmt=10,offset=this.offSet){
            let fetchUrl = `https://api.spacexdata.com/v3/launches?limit=${lmt}&offset=${offset}`

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
                            this.capsules = [...this.capsules, ...res];
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
        callCapsules(){
            let bottomOfWindow = Math.ceil(document.documentElement.scrollTop + window.innerHeight) === Math.round(document.documentElement.offsetHeight);
            //console.log(bottomOfWindow,document.documentElement.scrollTop,window.innerHeight,document.documentElement.offsetHeight/2)
            if(bottomOfWindow){
                this.addCapsules();
            }
        }
    }
}
</script>