<template>
    <div id="app">
        <button class="btn btn-lg btn-primary" @click="play=!play,file=`/static/demo.mp3`">

            {{label}}

        </button>
        <vue-audio :file="file"
                   :play="play"
                   @loading="loading"
                   @canplay="playing"
                   @pause="pause"

        ></vue-audio>
    </div>
</template>
<style lang="stylus">
    #app
        padding 10px
</style>
<script>
    import VueAudio from './component/VueAudio.vue'
    export default {
        name: 'app',
        components: {VueAudio},
        data () {
            return {
                play:false,
                msg: 'Welcome to Your Vue.js App',
                file:"",
                label:"play"
            }
        },
        methods:{
            loading(loading){
                this.label="loading"




            },
            playing(){
                this.label="pause"
            },
            pause(){
                this.label ="play"
            }
        }
    }
</script>

