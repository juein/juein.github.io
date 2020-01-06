---
title: "Gigagenie Web Container"
categories: work
date: 2019-06-25 12:00:00 -0400
image: 
  path: ../img/work_thumbnail/gigaginie.jpg
  thumbnail: ../img/work_thumbnail/gigaginie.jpg
  caption: ""
---

## 컨테이너 셋팅 및 구현

### [ADB (Androud Debug Bridge)]



- 기가지니와 PC의 연결은 ADB를 사용하여 연결한다.

- 설치 : 안드로이드 스튜디오를 설치
  URL : [https://developer.android.com/studio/ ](https://developer.android.com/studio/  "androidStudio")



#### adb 쉘 기본 명령어

adb 명령어는 cmd창에서 입력하면 된다.

adb 디바이스 확인 : **adb devices**  

adb 연결 : **adb connect `ipNum` : `portNumber` **  // ex ) adb connect 127.0.0.1:1234  

adb 연결해제 : **adb disconnect `ipNum  `**

adb kill : **adb kill-server**  



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_1.png)





### [Appmanager]

앱매니저 URL : `http:// ip : port /sdk/appmanager.html`

![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_2.png)

- URL 인풋에 웹앱의 url을 기입하고 Create 버튼을 누르면 adb 로 연결된 디바이스에 앱을 띄울 수 있다.
- 웹앱 종료시 Destroy 버튼을, 앱에서의 콘솔 확인시 Debug 버튼을 사용





### [기가지니 MC설정]

기가지니의 메인 컨트롤러(이하 MC) 설정 명령어 또한 adb로 한다.

adb connect 로 디바이스가 연결된 상태에서

```
adb shell am broadcast -a kt.action.mc.config
```

위 명령어 입력시 MC 설정창이 뜬다.

MC 설정에서는 기가지니의 대화모델을 설정 할 수 있다. (대화모델의 개발/상용 설정 등..)



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_3.png)



- 기가지니 단말에서 개발 중인 인텐트를 테스트 하고 싶은 경우에는

`대화서버 URL : 대화서버 개발자용(dialogkit, utf-8)`

`Container App 검증모드 : ALL TB`

으로 설정해야 개발중인 인테트를 인식할 수 있다.



- 반대로 상용화 된 대화모델만 인식하고 싶은 경우엔 개발중인 인텐트를 인식하지 않으며

`대화서버 URL : 대화서버 상용(utf-8) `

`Container App 검증모드 : ALL 상용`

으로 설정하면 개발중인 인텐트를 인식하지 않는다.





### [KT API Link]



기가지니의 인텐트 등록 및 설정의 작업들은 KT API 사이트에서 가능하다.

- KT API Link :[https://apilink.kt.co.kr/ ](https://apilink.kt.co.kr/  "apilink")

- 참고용 기가지니 개발 가이드 문서

  URL : [https://github.com/ktaidevelopers/GiGAGenie-Developers/wiki/GiGA-Genie-Developers-User-Guide ](https://github.com/ktaidevelopers/GiGAGenie-Developers/wiki/GiGA-Genie-Developers-User-Guide  "GiGAGenie-Developers")



계정 로그인 후 상위메뉴의 Console 페이지로 진입하면, 

My Service 탭에서 서비스 확인과 Service SDK , Dialog Kit에 진입할 수 있는 페이지가 뜬다.



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_4.png)





### [Dialog Kit]

Dialog Kit에서는 기가지니의 대화모델 (어휘 및 인텐트) 를 등록/수정 할 수 있다.

`KT API Link 의 Console 페이지에서 My Service -> Dialog Kit`페이지로 접근 가능하다.

- Dialog Kit 공식 가이드 문서 

  URL : [https://github.com/gigagenieDmt/DialogKit-deploymentGuide/wiki ](https://github.com/gigagenieDmt/DialogKit-deploymentGuide/wiki  "DialogKit-deploymentGuide")



#### 어휘

`Dialog Kit -> 대호모델 관리(DMT) -> 어휘사전 관리` 에서 어휘를 등록/수정 할 수 있다.

- 어휘사전명은 대문자만 입력 가능 (소문자 입력시 자동 치환)
- 어휘사전 목록은 엑셀파일로 정리 후 일괄 등록/다운이 가능하다.
- 어휘사전은 NE (명사)와 PR (동사) 타입으로 나누어진다. 



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_5.png)





#### 인텐트 등록

`Dialog Kit -> 대호모델 관리(DMT) -> 인텐트 관리` 에서 인텐트를 등록/수정 할 수 있다.

- 어휘사전에서 등록 된 어휘를 인텐트와 연결할 수 있다.

- 인텐트와 어휘 연결시, 어휘는 NE와 PR의 단독 사용이나, NE + PR 사용이 가능하다.

- 인텐트는 대/소문자를 구분한다.

- 인텐트 목록 또한 어휘사전 처럼 엑셀파일로 정리 후 일괄 등록/다운이 가능하다.

- 엑셀파일로 인텐트 등록시 인텐트명은 등록되지만, 해당 인텐트와 어휘간의 연결 (인텐트 규칙) 은 등록되지 않는다.

- 인텐트 등록 후 인텐트관리 페이지 하단에 대화모델 시뮬레이터로 추출 인텐트를 확인할 수 있다.

  

![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_6.png)

대화모델 시뮬레이터에서 등록된 문구 입력시 추출되는 인텐트를 확인 할 수 있다.





#### 인텐트 배포

어휘사전과 인텐트의 추가/수정 작업이 끝나면 반드시 수정된 내용을 배포 해야한다.

- `Dialog Kit -> 통합 시험 -> 배포관리` 에서 배포 할 내용이 있을경우 [배포요청] 버튼이 활성화 된다. (어휘/인텐트의 변경된 내용이 없을 경우엔 [배포요청] 버튼이 비활성화)



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_7.png)



- 배포요청 버튼을 누르면 배포 요청이 된다.

- 배포요청 처리 시간은 알 수 없으며 대체로 1~3분내외, 또는 2시간내외, 심히 늦을 경우엔 다음날 적용 되는 경우도 있다…(배포 완료 시간을 확인해보면 알수있음)

  ![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_8.png)

  

- 인텐트의 배포가 완료되면 시뮬레이터 를 사용할 수 있다.

- 시뮬레이터는 인텐트관리 페이지 하단의 대화모델 시뮬레이터 보다 더 상세한 로그를 확인할 수 있다.

- 시뮬레이터 사용시 상단의 APP 실행의 `예/아니오`는 앱 실행여부를 체크하는 단계, 
  등록된 앱의 시뮬레이터를 사용할 경우에는 반드시 `예` 를 선택해야 실행 된다.



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_9.png)





### [Service SDK]

Service SDK에서는 컨테이너의 URL설정과 Dialog Kit에서 등록된 인텐트의 유형정보 (컨테이너 타입 및 투명도 등)를 설정할 수 있다.

`Console 페이지 -> My Service -> Service SDK 정보 등록/수정` 버튼 클릭시 Service SDK 팝업이 활성화 된다.



**Invoke명** : 해당 서비스의 대표이름

**베이스 URL** : 컨테이너 위치의 URL을 기입

**인텐트정보** : Dialog Kit 에서 등록한 인텐트 리스트.  [추가] 버튼으로 인텐트 명을 추가할 수 있지만 여기서 추가된 인텐트 명은 ‘인텐트 명’ 만 추가되므로 어휘사전과의 대표단어 연결은 Dialog Kit 에서 작업 해야 한다.

**인텐트의 서비스 URL** : 컨테이너 URL 위치에서 해당 인텐트가 실행되는 URL을 기입

**Window 유형** : 컨테이너가 서비스 화면에 올려지는 타입

**Window 좌표** : top X, Y, Bottom X, Y의 값과 투명도를 설정

**Main Page 선택** : 메인 페이지의 인텐트를 선택



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_10.png)





### [컨테이너 소스]

- GiGa Genie Service API 공식문서

  URL : [https://github.com/GiGAGenie-ServiceSDK/UserGuide/wiki ](https://github.com/GiGAGenie-ServiceSDK/UserGuide/wiki  "GiGAGenie-ServiceSDK")

- 컨테이너와 웹앱간의 송/수신시 APP ID 와 기가지니 API Key 를 알고 있어야 한다.
  API Key는 KT API Link 사이트의 `Console -> My Service -> 페이지 하단의 [Key 보기]` 로 알 수 있다.

- KEY Type은 개발시 `GBOXDEVM`, 상용시 `GBOXCOMM ` 값을 사용하면 된다. (공식 문서 참조)

  

![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_11.png)



#### 컨테이너와 웹앱의 구현 모습



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_13.jpg)



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_14.jpg)



![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_15.jpg)





#### 자주 사용 된 API

**gigagenie.init** : 기가지니 서비스를 KEY TYPE 과 API KEY 값으로 초기화 한다. `result_cd` 값이 200이 아니면 API는 동작하지 않는다. 초기화 성공시 MR 버전, 디바이스 유형 같은 값을 받아온다.



**media.onRemoteKeyEvent** : JS Keycode가 없는 리모컨 키 이벤트를 수신 (리모컨의 `*`, `#` 키 등의 처리)



**media.onOTVWebAppMsg** : 웹앱에서 AndroidBridge를 통해 송신한 메시지를 받아오는(수신) 함수



**media.sendMsgToOTVWebApp** : 기가지니 컨테이너에서 웹앱으로 메시지를 송신(Send) 하는 함수



**voice.sendTTS** : 입력된 텍스트를 사용자에게 음성으로 전달해주는 함수



**voice.stopTTS** : 음성안내/인식을 중단하는 함수



**voice.getVoiceText** : 음성인식(녹음) 을 하는 함수

※ 음성 녹음의 경우 발화를 빠르게 해야 인식률이 높은 것 같다. 

발화를 천천히 하다보면 원치않은 결과가 나올때가 있다... 

![image](https://raw.githubusercontent.com/juein/juein.github.io/master/_posts/img/2019-06-25-genie_12.png)

( 010 발화시 공 10 으로 인식한다.  이런 경우 한글자는 인텐트 등록도 불가능하니 컨테이너 소스에서 별도처리를 한다.)



**voice.onActionEvent** : 기가지니에서 사용자가 발화(action)을 하면 KT API Link에서 등록 된 인텐트(event)를 수신해주는 함수



**voice.onVoiceCommand** : 기가지니의 공통키 (이전, 확인키) 등의 처리를 위한 함수



**voice.onRequestClose** : 서비스 종료 요청을 수신





- 서비스 초기 설정 가이드

  URL : [https://github.com/ktaidevelopers/GiGAGenie-Developers/wiki/%EC%84%9C%EB%B9%84%EC%8A%A4-%EC%B4%88%EA%B8%B0-%EC%84%A4%EC%A0%95 ](https://github.com/ktaidevelopers/GiGAGenie-Developers/wiki/%EC%84%9C%EB%B9%84%EC%8A%A4-%EC%B4%88%EA%B8%B0-%EC%84%A4%EC%A0%95  "GiGAGenie-Developers")



**HTML 페이지 (로그 확인용)**

```
<html>
	<head>
		<style>
			<!-- 컨테이너의 배경색을 지정. 투명도는 Service SDK에서 설정 필요 -->
			body{background: #fff;}   
		</style>
		<script type="text/javascript" src="https://svcapi.gigagenie.ai/sdk/v1.0/js/gigagenie.js"></script>
		<script src="js/container.js"></script>
	</head>
	</head>
    <body>
        <div id="log"></div>   <!-- 여기에 로그 내용을 출력-->
    </body>
</html>
```



**JS페이지 (containse.js)**

```
//서비스 초기화
var OTV_APP_ID;
var options={};
options.keytype="GBOXDEVM"; // 개발(GBOXDEVM) 또는 상용(GBOXCOMM) 키 종류 입력
options.apikey="개발/상용 키"; // 개발자 포털에서 키를 발급받아 입력

$(document).ready(function(){
    gigagenie.init(options,function(result_cd,result_msg,extra){
		if(result_cd == 200){
			//init 성공
			sendTTS("init success");
		};
    });
});

//TTS 송출 API 입력 Text를 사용자에게 음성으로 전달
function sendTTS(ttstext) {
	var options={};
	options.ttstext = ttstext;
	gigagenie.voice.sendTTS(options,function(result_cd,result_msg,extra) {
		if(result_cd == 200) {
		}
		else if(result_cd == 501){} // KWS에 의해 정지됨 (Pause/Resume)
		else if(result_cd == 503){} // StopTTS에 의해 정지됨 (Pause/Resume)
        $('#log').append('sendTTS result = ' + result_cd); // dom에 로그 찍기
	});
}

//웹앱으로 메시지 수신
function onMessage(extra){
    var test = JSON.stringify(extra);
    $('#log').empty();
    $('#log').append(test);
	
	gigagenie.media.onOTVWebAppMsg = function(extra){
		$('#log').append("onmessage 실행");
		OTV_APP_ID = extra.uAppId;
		sendMessage(OTV_APP_ID, "test send message", ""); //테스트 메시지 송신
	}
}

//웹앱으로 메시지 송신
function sendMessage(uAppId, message, args){
	options.uAppId = uAppId;
	options.message = message;
	options.args = args;
	gigagenie.media.sendMsgToOTVWebApp(options,function(result_cd,result_msg,extra){
	  if(result_cd===200){
		$('#log').append("Send Msg Success");
		$('#log').append(message);
	  }else {
		$('#log').append("Send Msg Fail");
	  }
	});
}

// 포털 내 Service SDK 정보 수정에서 작성한 서비스 URL보다 우선 순위로 실행됨
gigagenie.voice.onActionEvent = function(extra){
	$('#log').empty();
	$('#log').append(
		"<p>actioncode :" + extra.actioncode + "</p>" //발호 인텐트
		+"<p>uword :" + extra.uword + "</p>" //호출 인텐트명
		+"<p>voiceid :" + extra.voiceid + "</p>" // 컨테이너 id값
		+"<p>parameter  :" + JSON.stringify(extra.parameter) + "</p>" //파라미터값
	);

	// 인텐트 처리 
	switch(extra.actioncode){
		case 'test' : // 테스트 인텐트로 넘어올 경우 음성녹음 함수 실행
			var options={};
			options.delivery='memory';  // memory : js audio buffer , webhook : HTTP post
			options.recordTime = 10;  //녹음시간 (최대 5분)
			startRecordAudio(options);
			break;
		default :
			break;
	}
	$('#log').append('onAction 종료');
}

//음성 녹음 수신 API
function startRecordAudio(options){
	var audioBuffer = null;
	gigagenie.media.onVoiceRecordComplete=function(result_cd,inBuffer){
		if(result_cd===200) {
			$('#log').append('recording success');
			audioBuffer = inBuffer;
		} else{
			$('#log').append('recording fail:'+result_cd);
		}
		
		//AudioBuffer로 웹에서 출력하고자 하는 경우
		var source = context.createBufferSource();
		source.buffer=audioBuffer;
		source.connect(context.destination);
		source.start(0);
	}
	gigagenie.media.startRecordAudio(options,function(result_cd,result_msg,extra){
		$('#log').append(result_cd);
	});
}

//서비스 종료 명령 수신 API
gigagenie.voice.onRequestClose=function(){
	options={};
	gigagenie.voice.svcFinished(options,function(result_cd,result_msg,extra){
		$('#log').append('close');
	});
};
```



