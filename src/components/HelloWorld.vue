
<template>
  <!-- 
  AndroidManifest.xml配置設定
  <application android:hardwareAccelerated="true" android:icon="@mipmap/icon" android:label="@string/app_name" android:supportsRtl="true">
      <activity android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale" android:label="@string/activity_name" android:launchMode="singleTop" android:name="MainActivity" android:theme="@android:style/Theme.DeviceDefault.NoActionBar" android:windowSoftInputMode="adjustResize">
          <intent-filter android:label="@string/launcher_name">
              <action android:name="android.intent.action.MAIN" />
              <category android:name="android.intent.category.LAUNCHER" />
          </intent-filter>
          <intent-filter>
              <action android:name="android.intent.action.VIEW" />
              <category android:name="android.intent.category.DEFAULT" />
              <category android:name="android.intent.category.BROWSABLE" />
              <data android:host="lottery" android:scheme="gti" />
          </intent-filter>
      </activity>
  </application>
  -->
  <div class="hello">
    <h2 @click="submitFn()">click submitFn()</h2>
    <h2>
      <a href="gti://lottery">a href="gti://lottery"</a>
    </h2>
    <h2 @click="hrefOpen()">
       <a>click hrefOpen</a>
    </h2>
    <h2 @click="iframeOpen()">
      click iframeOpen
    </h2>
    <h2 ref="callClick" @click="callChromeIntent()"> call chrome intent</h2>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      iframe:null
    }
  },
  created(){

  },
  mounted(){
    //不管用哪種方法 在建構函數時呼叫 chrome都會擋住
    // this.callChromeIntent();
  },
  methods:{
    callChromeIntent(){
      console.log("callChromeIntent");
      var openTime = +new Date();
      window.location.href = "intent://lottery/#Intent;scheme=gti;package=com.example.hello;end";
      // var timer = setTimeout(function () {
      //     if ((new Date()) - openTime < 2200) {//加了200ms基准误差
      //         window.location.href = 'http://www.gt-igaming.com/?language=zh-Hant';
      //     }
      //     if ((new Date()) - openTime > 2200) {
      //         clearTimeout(timer);
      //     }
      // }, 2000);
    },
    hrefOpen(){
      var openTime = +new Date();
      window.location.href = 'gti://lottery'
      var timer = setTimeout(function () {
          if ((new Date()) - openTime < 2200) {//加了200ms基准误差
              window.location.href = 'http://www.gt-igaming.com/?language=zh-Hant';
          }
          if ((new Date()) - openTime > 2200) {
              clearTimeout(timer);
          }
      }, 2000);
    },
    iframeOpen()
    {
      this.iframe = document.createElement('iframe');
      this.iframe.src = 'gti://lottery';
      this.iframe.style.display = 'none';
      document.body.appendChild(this.iframe);
      // setTimeout(function() {
      //   // 执行成功后移除iframe
      //   document.body.removeChild(this.iframe);
      //   //setTimeout小于2000通常认为是唤起APP失败 
      //   if (Date.now() - last < 2000) {
      //           // 执行失败函数
      //           // 这里需要考虑一下之前知乎遇到的那个问题（浏览器询问导致时间小于2S）
      //   } 
      //   else {

      //           //  执行成功函数

      //   }
      // }, timer);

    },
    submitFn(){
      console.log("click");
      //判断浏览器
      var u = navigator.userAgent;
      var d = new Date();
      var t0 = d.getTime();
      if(u.indexOf('Android') > -1 || u.indexOf('Linux') > -1){
          //Android
          if(this.openApp('gti://lottery')){
            this.openApp('gti://lottery');
          }else{
              //由于打开需要1～2秒，利用这个时间差来处理－－打开app后，返回h5页面会出现页面变成app下载页面，影响用户体验
              var delay = setInterval(function(){
                  var d = new Date();
                  var t1 = d.getTime();
                  if( t1-t0<3000 && t1-t0>2000){
                      alert('请下载APP');
                      window.location.href = "http://www.gt-igaming.com/?language=zh-Hant";
                  }
                  if(t1-t0>=3000){
                      clearInterval(delay);
                  }
              },1000);
          }
      }
      else if(u.indexOf('iPhone') > -1){
          //IOS
          if(this.openApp('ios--scheme')){  
              this.openApp('ios--scheme');
          }else{
              var delay = setInterval(function(){
                  var d = new Date();
                  var t1 = d.getTime();
                  if( t1-t0<3000 && t1-t0>2000){
                      alert('请下载APP');
                      window.location.href = "app下载地址";
                  }
                  if(t1-t0>=3000){
                      clearInterval(delay);
                  }
              },1000);
          }
      }    
    },
    openApp(src) {
    // 通过iframe的方式试图打开APP，如果能正常打开，会直接切换到APP，并自动阻止a标签的默认行为
    // 否则打开a标签的href链接
        let ifr = document.createElement('iframe');
        ifr.src = src;
        ifr.style.display = 'none';
        document.body.appendChild(ifr);
        window.setTimeout(function(){
              document.body.removeChild(ifr);
        },2000);
    }

  }
}
</script>

<style scoped>
h2 {
  font-weight: normal;
  cursor: pointer;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
