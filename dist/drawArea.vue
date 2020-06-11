<template>
    <div :class="{drawAreaContainer:true,ScrollViewDrawUp:ScrollViewDrawUp,ScrollViewDrawDown:ScrollViewDrawDown}">
        <span class="tip" v-if="isNoMore||show">
            {{tip}}
        </span>
        <svg v-else t="1591254865122" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"
             p-id="1196">
            <path d="M523.085935 101.849403m-101.850403 0a101.850403 101.850403 0 1 0 203.700806 0 101.850403 101.850403 0 1 0-203.700806 0Z"
                  :fill="color" p-id="1197"></path>
            <path d="M769.836489 187.508901m-96.031437 0a96.031437 96.031437 0 1 0 192.062875 0 96.031437 96.031437 0 1 0-192.062875 0Z"
                  :fill="color" p-id="1198"></path>
            <path d="M903.286707 381.395765m-90.210471 0a90.210471 90.210471 0 1 0 180.420943 0 90.210471 90.210471 0 1 0-180.420943 0Z"
                  :fill="color" p-id="1199"></path>
            <path d="M905.950692 609.722427m-84.390506 0a84.390506 84.390506 0 1 0 168.781011 0 84.390506 84.390506 0 1 0-168.781011 0Z"
                  :fill="color" p-id="1200"></path>
            <path d="M799.997313 786.127394m-78.57054 0a78.57054 78.57054 0 1 0 157.141079 0 78.57054 78.57054 0 1 0-157.141079 0Z"
                  :fill="color" p-id="1201"></path>
            <path d="M605.196454 889.708787m-72.750574 0a72.750574 72.750574 0 1 0 145.501148 0 72.750574 72.750574 0 1 0-145.501148 0Z"
                  :fill="color" p-id="1202"></path>
            <path d="M397.148673 877.857856m-66.931608 0a66.931608 66.931608 0 1 0 133.863216 0 66.931608 66.931608 0 1 0-133.863216 0Z"
                  :fill="color" p-id="1203"></path>
            <path d="M223.665689 762.483532m-61.110641 0a61.110642 61.110642 0 1 0 122.221283 0 61.110642 61.110642 0 1 0-122.221283 0Z"
                  :fill="color" p-id="1204"></path>
            <path d="M134.483212 587.14856m-55.290676 0a55.290676 55.290676 0 1 0 110.581352 0 55.290676 55.290676 0 1 0-110.581352 0Z"
                  :fill="color" p-id="1205"></path>
            <path d="M135.396207 408.896604m-49.47071 0a49.47071 49.47071 0 1 0 98.94142 0 49.47071 49.47071 0 1 0-98.94142 0Z"
                  :fill="color" p-id="1206"></path>
            <path d="M205.336797 260.047476m-43.650744 0a43.650744 43.650744 0 1 0 87.301488 0 43.650744 43.650744 0 1 0-87.301488 0Z"
                  :fill="color" p-id="1207"></path>
            <path d="M315.81515 159.990063m-37.829779 0a37.829778 37.829778 0 1 0 75.659557 0 37.829778 37.829778 0 1 0-75.659557 0Z"
                  :fill="color" p-id="1208"></path>
        </svg>
    </div>
</template>

<script>
    export default {
        name: "drawArea",
        props:{
            color:{
                default:'grey',
                type:String
            },
            tip:{
                type:String,
                default: ''
            },
            type:{
                default:'top',
                type:String
            },
            isNoMore:{
                default:false,
                type:Boolean,
            }
        },
        created() {
            if(this.type==='top'){
                this.show=true
            }
        },
        mounted() {
            let node=this.$el
            if(this.type=='top'){
                node.style.bottom='100%'
            }else if(this.type=='bottom'){
                node.style.top='100%'
            }
            console.log(node);
        },
        methods: {
            toggleTip(bool) {
                this.show=bool
            }
        },
        data(){
            return{
                show:false,
                ScrollViewDrawDown:false,
                ScrollViewDrawUp:false,
            }
        }
    }
</script>

<style scoped>
    .drawAreaContainer{
        z-index: 999;
        position: absolute;
        padding:15px 0;
        width: 100%;
        background:transparent;
        box-sizing: border-box;
        transition: translateY .5s;
    }
    .icon{
        max-height: 5vw;
        animation:cycle .8s infinite;
    }
    @keyframes cycle {
        from{
            transform: rotate3d(0,0,0,0);
        }
        to{
            transform: rotate3d(0,0,1,360deg);
        }
    }

</style>