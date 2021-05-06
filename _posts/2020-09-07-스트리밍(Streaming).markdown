---
title: 스트리밍(Streaming)
date: 2020-09-07 18:14:00
categories:
 - DEFINITION
tag:
 - Streaming
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

### 스트리밍이란?

스트리밍이란 인너넷(네트워크)을 바탕으로 사용자들에게 각종 비디오, 오디오 등의 멀티미디어 디지털 정보를 제공하는 기술로 인터넷에서 영상 및 음향 등의 파일을 하드디스크 드라이브에 다운로드 받아 재생하던 것을 다운로드 없이 실시간으로 재생해 주는 기법이다.

전송되는 데이터가 물 흐르듯 처리된다고 해서 "스트리밍(Streaming)"이라 표현하고, 파일이 모두 전송되지 않아도 브라우저 또는 플러그인이 데이터를 표현하기 시작한다. 재생 시간이 단축되며 클라이언트 하드디스크 드라이브의 용량에도 영향을 받지 않는다.

### 라이브 스트리밍 프로토콜 종류

#### 기존 프로토콜

- RTSP(Real-Time Streaming Protocol)
- RTMP(Real-Time Messaging Protocol)

#### HTTP-Based Adaptive Protocols

- Apple HLS(HTTP Live Streaming)
- Low-Latency HLS
- MPEG-DASH (Moving Picture Expert Group Dynamic Adaptive Streaming over HTTP)
- Low-Latency CMAF for DASH (Common Media Application Format for DASH)
- Microsoft Smooth Streaming
- Adobe HDS(HTTP Dynamic Streaming)

#### New Technologies

- SRT(Secure Reliable Transport)
- WebRTC(Web Real-Time Communications)

기본적으로 라이브 스트리밍 프로토콜을 사용하는 스트리밍 서버는 영상 데이터 전송뿐만 아니라, 동영상에 대한 정보 분석이나 전송 규격에 맞도록 동영상 파일을 읽어서 변형하는 기능도 갖춰야 한다. RTSP/RTP 의 경우 서로 다른 네트워크 연결로 데이터 교환이 발생하기 때문에 NAT, 방화벽 등의 환경에서 서비스가 원활하지 않다.

반면 HTTP 전송 채널로 사용하는 경우 웹 서비스를 위한 캐시 구조와 기존의 CDN(Content Delivery Network)를 이용할 수 있다.

