<template>
  
    <div style="height:100%;width:98%">
        <el-container >
            <el-header> <span>北京邮电大学下一代中心转码系统展示平台</span></el-header>         
        </el-container>

        <el-container>
            <el-header style="margin-top: 0px;height:98%;width:98%;">
                <div style="margin-top: 0px;">
                    <el-input placeholder="请输入内容" v-model="input1" class="input-with-select" >
                        <el-select v-model="select" slot="prepend" placeholder="请选择码率">
                            <el-option label="源资源" value="0"></el-option>
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
            <el-header><span>北京邮电大学下一代中心转码系统展示平台</span></el-header>
            <el-container>               
                <el-main  >
                    
                    <video-player class="vjs-custom-skin"    
                        ref="videoPlayer"
                        :options="playerOptions"
                        :playsinline="true"
                        customEventName="customstatechangedeventname"
                        @play="onPlayerPlay($event)"
                         @loadeddata="onPlayerLoadeddata($event)"  
                    >
                    </video-player>
                </el-main>
                <el-aside style="width:30%" >
                    <el-tabs type="border-card"  >
                        <el-tab-pane label="audio" style="border-collapse:separate; border-spacing:0px 0px;">
                            <span>
                                <tr >delay:{{timegap}}ms</tr>
                                <tr >bitrate: </tr>  
                                <div >index: </div>                                  
                                <div>pendingIndex: </div> 
                                <tr >ratio: </tr> 
                                <tr >download: </tr>
                                <tr >latency: </tr> 
                                <tr >droppedFPS: </tr>
                                <tr >liveLatency: </tr>     
                            </span>                  
                        </el-tab-pane>
                        <el-tab-pane label="video">
                                <tr >buffer: </tr>
                                <tr >bitrate: </tr>  
                                <tr >index: </tr>                                  
                                <tr >pendingIndex: </tr> 
                                <tr >ratio: </tr> 
                                <tr >download: </tr>
                                <tr >latency: </tr> 
                                <tr >droppedFPS: </tr>
                                <tr >liveLatency: </tr>                          
                        </el-tab-pane>
                        </el-tabs>                     
                </el-aside>
            </el-container>
        </el-container>
        

        <el-container style="height:300px">
            <el-main >
            <img  src="../assets/piont.png" style="height:95%;width:700px">
            </el-main>
            <el-footer><span>北京邮电大学下一代中心转码系统展示平台</span></el-footer>
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
      },
     onPlayerLoadeddata(data) {
          this.timerecive = new Date();
          this.timegap=this.timerecive-this.timebegin
       console.log('player Loadeddata!', this.timerecive)
       console.log('player gap!', this.timegap+'ms')
       console.log('player data ' ,data)
     },
    //   onPlayerWaiting(player) {
    //      console.log('player Waiting!', player)
    //   },
    //   onPlayerPlaying(player) {
    //      console.log('player Playing!', player)
    //   },
    //   onPlayerTimeupdate(player) {
    //      console.log('player Timeupdate!', player.currentTime())
    //   },
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

