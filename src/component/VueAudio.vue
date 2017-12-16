
<script>

    export default {
        name: 'vue-audio',
        render(){},
        props: {
            file: String,
            play:Boolean
        },
        watch:{
            source(src){
                this.audio.src =src
            },
            playing(playing){

//                this.$emit("play")
            },
            shouldPlay(play){
                console.log(play,"play");
                if(this.playing){
                    this.doPause()
                    this.$emit("pause")
                }else{
                    if(play){

                        if(!this.loaded){
                            this.load();
                        }else{
                            this.doPlay()
                        }
                    }
                }
            }
        },
        computed: {
            shouldPlay(){
                return this.play
            },
            source (){
                return this.file
            },
        },
        data () {
            return {
                loaded: false,
                playing: false,
                paused: true,
                audio: undefined,
            }
        },
        methods: {
            init () {
                this.audio = new Audio();
                this.audio.addEventListener('loadeddata', this.handleLoadedEvent);
                this.audio.addEventListener('pause', this.handlePlayPauseEvent);
                this.audio.addEventListener('play', this.handlePlayPauseEvent);
            },
            load(){
                this.audio.load();
                this.$emit("loading")
            },
            doPlay () {
                if (this.playing) return
                this.paused = false
                this.audio.play()
                this.playing = true
            },
            doPause () {
                this.paused = !this.paused;
                (this.paused) ? this.audio.pause() : this.audio.play()
            },

            handleLoadedEvent () {
                console.log(1111)
                if (this.audio.readyState >= 2) {
                    this.doPlay();
                    this.loaded = true
                    this.$emit("canplay",true);
                } else {
                    this.load();
                }
            },
            handlePlayPauseEvent (e) {
                if (e.type === 'pause' && this.playing === false) {
                    this.paused = true
                }
            },
        },
        mounted () {
            this.$nextTick(()=>{
                this.init()
            })
        },
        beforeDestroy () {
            this.audio.removeEventListener('loadeddata', this.handleLoadedEvent)
            this.audio.removeEventListener('pause', this.handlePlayPauseEvent)
            this.audio.removeEventListener('play', this.handlePlayPauseEvent)
        }

    }



</script>

