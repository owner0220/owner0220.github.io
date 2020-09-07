---
title: HLS(Http Live Streaming)
date: 2020-09-07 18:14:00
categories:
 - Http
tag:
 - Streaming
 - HLS
---

> 스트리밍의 종류
>
> - HLS(Http Live Streaming)
>
> - Progressive Download
> - Pseudo Streaming
> - RTSP, RTMP
> - Adaptive Streaming
> - Http Dynamic Streaming

### HLS(Http Live Streaming) 이란?

Apple(iPhone, iPad 등) 에서 사용하는 표준 HTTP 기반 스트리밍 프로토콜이다. 프로토콜에서 스트리밍 데이터를 m3u8 의 확장자를 가진 재생목록 파일과 잘게 쪼개놓은 다수의 ts 파일들(동영상)을 HTTP 를 통해 전송하는 방식을 사용한다.

– m3u8 : m3u 파일인데, UTF-8 로 인코딩 되어 있다는 의미.

– m3u : 멀티미디어 파일의 재생목록을 관리하는 파일

– ts : MPEG-2 의 Transport Stream 포맷

HLS는 iPhone과 iPad의 사용자 수가 늘어남으로써 자연스럽게 그 수요가 늘어나게 되었다. 또한, 규격 자체의 단순함과 IETF(Internet Engineering Task Force)를 통한 표준화 작업 등을 통해 다른 업체들도 쉽게 HLS를 지원할 수 있게 했다. 그 결과로, Adobe는 Flash Media Server 4.0에서, Microsoft는 IIS Media Server 4.0에서 HLS를 정식으로 지원하며, 모바일 운영체제에서 상대 진영이라 할 수 있는 Google의 Android에서도 3.0 버전인 Honeycomb부터 HLS를 지원하기 시작했다.

HLS 전송 방식

HLS에서 서버는 HTTP 를 통해 클라이언트로부터 요청을 받고, 데이터에 대한 응답을 주는 역할만 수행하기 때문에, 저장되어 있는 파일을 읽어서 HTTP 응답에 데이터를 실어서 보낼 수 있는 어떤 웹 서버든지 사용이 가능하다. 스트림 세그먼터 (Stream Segmenter) 는 일정한 시간 간격마다 입력 받은 미디어 데이터를 분할해 파일로 만들고, 그 분할한 파일에 접근할 수 있는 메타데이터(m3u8) 를 생성하는 역할을 한다. HTTP 는 양방향 방식이 아니기 때문에, 클라이언트에서 반드시 서버에 요청을 해야만 응답을 받을 수 있다.

[
![Apple Inc, HTTP Live Streaming Overview](https://blog.kollus.com/wp-content/uploads/2014/05/Apple-Inc-HTTP-Live-Streaming-Overview-.jpg)](https://blog.kollus.com/wp-content/uploads/2014/05/Apple-Inc-HTTP-Live-Streaming-Overview-.jpg)

\* Apple Inc, HTTP Live Streaming Overview (출처 http://developer.apple.com/library/ios/)

HLS 에서는 ABS(Adaptive Bitrate Streaming) 를 위해 동시에 여러 비트율의 ts 파일에 대한 정보를 제공한다. ABS 을 지원하기 위한 m3u8 파일과 ts 파일 구조는 아래 그림과 같다. 전체를 대표하는 m3u8 파일이 있고, 대표 파일 내 각각의 비트율별 플레이 리스트 파일을 가리키게 한다. 각 비트율별 플레이 리스트 파일은 다시 각 비트율에 해당하는 ts 파일을 가리킨다.
HLS는 확장성이 높고 안정적이기는 하나, 이러한 구조상 파일을 먼저 만들고 스트리밍하는 방식이기 때문에 RTMP/RTSP 방식에 비해 딜레이 문제가 발생할 수 있다.

[![ABS를 위한 m3u8 파일과 ts 의 연결구조](https://d2.naver.com/content/images/2015/06/helloworld-7122-10.png)]()

\* ABS를 위한 m3u8 파일과 ts 의 연결구조

(출처 http://helloworld.naver.com/helloworld/7122 )

HLS는 기존의 스트리밍 프로토콜이 지닌 단점을 극복하고 스트리밍 서비스를 위한 인프라를 단순화시키려는 의도에서 나온 규격이라고 할 수 있다. 고화질 서비스를 지원하고, 네트워크 상황에 따라 원하는 비트레이트의 콘텐츠를 선택할 수 있는 기능을 제공한다. Android 3.0 기기부터는 공식적으로 HLS를 지원하여 iOS 기기 및 Android 기기에 모두 동일한 방식의 라이브 스트리밍 서비스를 할수 있게 되었다. HLS 이외에도 HTTP 를 이용한 Adaptive Bitrate Streaming을 지원하는 스트리밍 규격이 있다. Adobe의 HTTP Dynamic Streaming과 Microsoft의 Smooth Streaming도 HTTP를 이용한 스트리밍 규격이다.

ABS (Adaptive Bitrate Streaming)

– 사용자의 네트워크 속도에 따라 적합한 콘텐츠를 선택하여 재생

– 사용자 별 서비스 품질 다변화

– 대역폭 변동에 대한 품질개선 및 안정성 확보

– Apple : HTTP Live Streaming (HLS)

– Adobe : HTTP Dynamic Streaming (HDS)

– Microsoft : HTTP Smooth Streaming (HSS)

————————————————————————————————————————

\* 출처

https://blog.kollus.com/?p=131

http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming
http://helloworld.naver.com/helloworld/7122
http://www.adobe.com/kr/products/hds-dynamic-streaming.html
http://www.thekuroko.com/what-is-http-dynamic-streaming/
http://www.jwplayer.com/blog/what-is-video-streaming/
http://blog.edgecast.com/post/55198896476/hds-hls-hss-adaptive-http-streaming-demystified
http://unipro.tistory.com/121
http://blog.naver.com/makerslee?Redirect=Log&logNo=30148808093
http://blog.naver.com/pyoyr?Redirect=Log&logNo=50037791019