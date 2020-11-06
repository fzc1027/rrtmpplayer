<template>

    <div style="height:100%;width:98%">
        <el-container >
            <el-header> <span>Transcoding system display platform</span></el-header>
        </el-container>

        <el-container>
            <el-header style="margin-top: 0px;height:98%;width:98%;">
                <div style="margin-top: 0px;">
                    <el-input placeholder="please input rtmp_address" v-model="input1" class="input-with-select" >
                        <el-select v-model="select" slot="prepend" placeholder="please select bit_rate">
                            <el-option label="resource" value="0"></el-option>
                            <el-option label="360p" value="1"></el-option>
                            <el-option label="720p" value="2"></el-option>
                            <el-option label="1080p" value="3"></el-option>
                        </el-select>
                         <el-button slot="append" icon="el-icon-search" @click="getInputValue"></el-button>
                    </el-input>
                </div>
            </el-header>
        </el-container>

        <el-container style="height:450px">
            <el-header><span>Transcoding system display platform</span></el-header>
            <el-container>
                <el-main  >

                    <video-player class="vjs-custom-skin"
                        ref="videoPlayer"
                        :options="playerOptions"
                        :playsinline="true"
                        customEventName="customstatechangedeventname"
                        @play="onPlayerPlay($event)"
                        @loadeddata="onPlayerLoadeddata($event)"
                        @playing="onPlayerPlaying($event)"
                        @pause="onPlayerPause($event)"
                        @ended="onPlayerEnded($event)"
                    >
                    </video-player>
                </el-main>
                <el-aside style="width:30%" >
                    <el-tabs type="border-card"  >
                        <el-tab-pane label="audio" style="border-collapse:separate; border-spacing:0px 0px;">
                            <span>
                                <tr >startup delay: {{timegap}} ms</tr>
                                <tr >downlink: {{downlink}} KB/s</tr>
                                <tr >bindwidth: {{bindwidth}} Kbps</tr>
                                <tr >latency: {{delay}} ms</tr>
                                <tr >bitrate: 1562.45 kbps </tr>
                                <div >index: 156</div>
                                <div>pendingIndex: 157 </div>
                                <tr >ratio: 1920×1080 </tr>
                                <tr >download: 1843.25 kbps </tr>
                                <tr >latency: 1854 ms </tr>
                                <tr >droppedFPS: 0 % </tr>
                                <tr >liveLatency: 1148 ms </tr>
                            </span>
                        </el-tab-pane>
                        <el-tab-pane label="video">
                                <tr >delay: {{timegap}} ms</tr>
                                <tr >bitrate: 1562.45 kbps </tr>
                                <div >index: 156</div>
                                <div>pendingIndex: 157 </div>
                                <tr >ratio: 1920×1080 </tr>
                                <tr >download: 1843.25 kbps </tr>
                                <tr >latency: 1854 ms </tr>
                                <tr >droppedFPS: 0 % </tr>
                                <tr >liveLatency: 1148 ms </tr>
                        </el-tab-pane>
                        </el-tabs>
                </el-aside>
            </el-container>
        </el-container>


        <el-container style="height:300px">
            <el-main >
            <img  src="../assets/piont.png" style="height:95%;width:95%">
            </el-main>
            <el-footer><span>Transcoding system display platform</span></el-footer>
        </el-container>


    </div>
</template>

    <style>

        .el-header, .el-footer {
            background-color: #FFFFFF;
            color: #333;
            text-align: left;
            line-height: 35px;
        }

        .el-container{
            height:100%;
            padding:0;
            margin:0;
            width:98%;
        }

        .el-aside, .el-tabs {
            background-color: #FFFFFF;
            color: #333;
            text-align: left;
            line-height: 30px;
            height:98%;
            width:98%;
        }

        el-image{
            background-color: #FFFFFF;
            color: #333;
            text-align: center;
            line-height: 30px;
            height:98%;
            width:98%;
        }




        .el-main {
            background-color: #FFFFFF;
            color: #333;
            text-align: center;
            line-height: 35px;
            width:100%;
            height:98%;
        }




        .el-select .el-input {
            width: 300px;

        }
        .input-with-select .el-input-group__prepend {
            background-color: #fff;
        }

    </style>




    <script>
    import 'videojs-flash'
    //import rplay from '@/views/rplay'
        export default {
         // components:{
         //   'rplay':rplay
         // },

            data() {
                return {
                    input1: '',
                    select: '',
                    timebegin:'',
                    timerecive:'',
                    timegap:'',
                    downlink:'',
                    bindwidth:'',
                    delay:'',
                    flag:0,
                    //play播放器参数设置
                    playerOptions: {
                        height: '340',
                        width: '100%',
                        sources: [ {
                        type: 'rtmp/mp4',
                        src: ''          //播放流地址，getInputValue函数用动态修改src地址
                        } ],
                        techOrder: ['flash'],
                        autoplay: false, // 非自动播放
                        // controls: true, // 控制条
                        isVideo:true,//强制刷新使用
                        // preload: 'auto', // 预加载
                        // muted: true, // 消除所有音频
                        // loop: false, // 循环播放
                        // controlBar: {
                        //         timeDivider: true,
                        //         durationDisplay: true,
                        //         remainingTimeDisplay: false,
                        //         fullscreenToggle: true  //全屏按钮
                        // }
                    }
                }
            },

            methods: {
                //在搜索按钮处插入的函数
                getInputValue(){

                    //设法找到转换码率select的内容，读取select只能得到选项123，故switch修改一下
                    switch (this.select) {
                    case '0':
                        this.bitrate='';
                    break;
                    case '1':
                        this.bitrate='_360';
                    break;
                    case '2':
                        this.bitrate='_720';
                    break;
                    case '3':
                        this.bitrate='_1080';
                    break;
                    default:
                        this.bitrate=''

                    }
                    this.url = this.input1+this.bitrate;  //拼接流地址

                    this.playerOptions['sources'][0]['src'] =this.url; //动态更改流地址

                    alert(this.url)  //显示流地址
                },
          //listen event
      onPlayerPlay() {
          this.timebegin = new Date();
       console.log('player play!', this.timebegin)
       this.flag=1;
       this.measureBW((speed,ping,bindwidth)=>{
            console.log(speed + " KB/sec");  //913 KB/sec
            console.log(ping + " ms");  //913 KB/sec
            console.log(bindwidth + " kps");  //913 KB/sec
            this.downlink=speed;
            this.bindwidth=bindwidth;
            this.delay=ping;
        },10)

      },
      onPlayerPause(player) {
         console.log('player pause!', player)
         this.flag=0;
      },
      onPlayerEnded(player) {
         console.log('player end!', player)
         this.flag=0;
      },
    onPlayerLoadeddata(data) {
        this.timerecive = new Date();
        this.timegap=this.timerecive-this.timebegin
        console.log('player Loadeddata!', this.timerecive)
        console.log('player gap!', this.timegap+'ms')
        console.log('player data ' ,data)
        console.log('是否在线',navigator.onLine)
        console.log('网速',navigator.connection.downlink,'MB/s')
    },
    // onPlayerWaiting(player) {
    //     console.log('player Waiting!', player)
    //     this.flag=0;
    // },
    onPlayerPlaying(player) {
        console.log('player Playing!', player);
        console.log('是否在线',navigator.onLine)
        console.log('网速',navigator.connection.downlink,'MB/s')
    },
    // onPlayerTimeupdate(player) {
    //     console.log('player Timeupdate!', player.currentTime())
    //     console.log('是否在线',navigator.onLine)
    //     console.log('网速',navigator.connection.downlink,'MB/s')
        // measureBW((speed,ping,bindwidth)=>{
        //     console.log(speed + " KB/sec");  //913 KB/sec
        //     console.log(ping + " ms");  //913 KB/sec
        //     console.log(bindwidth + " kps");  //913 KB/sec
        //     this.downlink=speed;
        //     this.bindwidth=bindwidth;
        //     this.delay=ping;
        // },10)
    // },

    measureBW(fn,time) {
    time = time || 1;
    var startTime, endTime, fileSize;
    var count = time ;
    var _this = this;
        function measureBWSimple () {
            var xhr = new XMLHttpRequest();
            xhr.onreadystatechange = () => {
                if (xhr.readyState === 4 && xhr.status === 200) {
                    if(!fileSize){
                        fileSize = xhr.responseText.length;
                    }
                    count --;
                    if(count<=0){
                        endTime = Date.now();
                        console.log(fileSize)
                        var speed = fileSize * time  / ((endTime - startTime)/1000) / 1024;
                        var bindwidth =speed*8
                        var ping = (endTime - startTime) / time ;
                        fn && fn(Math.floor(speed),Math.floor(ping),Math.floor(bindwidth));
                    }
                }
            }
            xhr.open("GET", "https://upload.wikimedia.org/wikipedia/commons/5/51/Google.png?" + Math.random(), true);
            xhr.send();
        }
    startTime = Date.now();
    for(var x = time;x>0;x--){
        measureBWSimple()
    }
    setTimeout(function(){

        console.log("gduyagsdgaysdgyasdasdsa",_this.flag)
        if(_this.flag==1){
        _this.measureBW(fn,10)
        }
    },1000)
}


    //   onPlayerCanplay(player) {
    //      console.log('player Canplay!', player)
    //   },
    //             // or listen state event
    //             playerStateChanged(playerCurrentState) {
    //               console.log('player current update state', playerCurrentState)
    //             },
            // player is ready


    }

    }


    </script>

