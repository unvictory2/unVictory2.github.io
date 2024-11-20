---
layout : single
title : " [네프 플젝] 프로그램 동작과 프론트 백 상호작용 정리"
excerpt : "언제 어떤 소캣을 열고, 각 데이터는 어떻게 저장하고 등"
published : true

categories : 
    - Network Programming

toc : true
toc_sticky: true

date : 2024-11-20
last modified : 2024-11-20
---

## 서론  
자바 소캣을 기반으로 하는 네트워크 프로그래밍 프로젝트로 게임을 만들게 됐다. 내 역할은 서버쪽이다.

<br>

## 흐름 정리
게임의 흐름을 먼저 정리해보자.

1. 서버 접속 및 대기방 입장
   - 플레이어가 서버에 접속하면 자동으로 대기방에 입장
   - 충분한 플레이어가 모일 때까지 대기
   - 대기 중 채팅 가능

2. 게임 시작 및 역할 분배
   - 충분한 플레이어가 모이면 게임 시작
   - 플레이어들이 시민과 라이어로 나뉨
   - 주제와 제시어 설정 (예: 주제 - 스포츠, 제시어 - 축구)
   - UI에 각 플레이어를 나타내는 버튼 표시

3. 제시어 설명 단계
   - 모든 플레이어가 돌아가며 제시어에 대해 설명
   - 각 플레이어에게 정해진 시간 제공 (타이머 표시)
   - 시민: 라이어가 알아차리지 못하도록 조심스럽게 설명
   - 라이어: 시민인 척 그럴듯하게 설명
   - 채팅 시 이모티콘 사용 가능

4. 자유 토론 시간
   - 플레이어들이 누가 라이어인지 추리하고 논의
   - 과거 각 인원의 제시어 설명 내용 확인 가능 (정보 버튼 사용)
   - 제한 시간 적용

5. 투표 단계
   - 라이어로 의심되는 플레이어에 대해 투표
   - 제한 시간 적용

6. 최후변론 및 제거 결정
   - 지목된 플레이어의 최후변론 (음성으로 진행 가능)
   - 지목된 플레이어만 말할 수 있음
   - 다른 플레이어들이 제거 여부 결정
   - 제한 시간 적용

7. 결과 확인
   - 제거된 플레이어가 시민일 경우: 게임 계속, UI에서 해당 플레이어 버튼에 X 표시
   - 제거된 플레이어가 라이어일 경우: 제시어 맞히기 기회 제공
     - 라이어가 맞히면: 라이어 승리
     - 라이어가 못 맞히면: 시민 승리

8. 밤 단계
   - 라이어들끼리 제시어에 대해 논의
   - 시민들은 대기
   - 제한 시간 적용

9. 다음 라운드로 반복 (3번 단계로 돌아감) 또는 게임 종료

10. 게임 종료 후 대기방으로 복귀
    - 플레이어들이 자동으로 대기방으로 돌아감
    - 새로운 게임 시작을 기다림

### 추가적인 특징:
- 각 단계마다 타이머가 적용되어 제한 시간이 끝나면 자동으로 다음 단계로 넘어감
- UI에는 현재 주제, 제시어(시민에게만 표시), 타이머, 채팅 창, 입력 필드 등이 포함됨
- 플레이어의 상태(현재 플레이어, 사망 여부 등)가 UI에 반영됨
- 게임 진행 중 언제든지 과거 대화 내용을 확인할 수 있는 기능 제공
- 대기방에서 플레이어 수, 준비 상태 등을 확인할 수 있는 UI 제공

### 나중에 추가를 고려해볼 사항들:
1. 그림을 포함할 방안 
- 제시어를 그림으로 제시
- 제시어를 그림으로 표현
- 사망시 피자국 같은 그림 전송

2. 추가적인 요소
- 자동 매칭
- 친구 목록 (데베 연동 필요)

<br>


## 서버 측면에서 게임의 각 단계별로 필요한 처리와 구현 방법 정리

1. 서버 초기화 및 대기 - 기존 수업 내용과 같음
   - 서버 소켓을 생성하고 지정된 포트에서 리스닝 시작 > 포트 지정
   - acceptThread를 생성하여 클라이언트의 접속을 대기 ServerSocket.accept()
   - Vector<ClientHandler>를 생성하여 연결된 클라이언트들을 관리

2. 클라이언트 접속 처리
   - 클라이언트 접속 시 ClientHandler 객체 생성 및 Vector에 추가 
   - ClientHandler 스레드 시작 (클라이언트와의 통신 담당) > 스레드 생성
   - 대기방 인원 수 확인 및 게임 시작 가능 여부 체크

3. 게임 시작 준비
   - 충분한 인원이 모이면 게임 시작 신호를 모든 클라이언트에 전송 ObjectOutputStream
   ```java
   if (users.size() >= MIN_PLAYERS) {
    ChatMsg startMsg = new ChatMsg("SERVER", ChatMsg.MODE_GAME_START);
    broadcasting(startMsg);
   }
   ```
   - 플레이어 역할 분배 (시민, 라이어)
   ```java
   class Player {
       String id;
       boolean isAlive;
       boolean isSpeaking;
       boolean isLiar;
       // 기타 필요한 정보들
   }

   List<Player> players = new ArrayList<>();
   for (ClientHandler client : users) {
      Player player = new Player(client.getUid());
      players.add(player);
   }

   Random random = new Random();
   int liarIndex = random.nextInt(players.size());
   players.get(liarIndex).setLiar(true);
   ```
   - 주제와 제시어 선정
   ```java
   for (int i = 0; i < players.size(); i++) {
      Player player = players.get(i);
      ClientHandler client = users.get(i);
      
      ChatMsg roleMsg = new ChatMsg("SERVER", ChatMsg.MODE_ROLE_ASSIGN);
      roleMsg.setRole(player.isLiar() ? "LIAR" : "CITIZEN");
      roleMsg.setTopic(selectedTopic);
      if (!player.isLiar()) {
         roleMsg.setWord(selectedWord);
      }
      
      client.send(roleMsg);
   }
   ```

4. 게임 진행
   a. 제시어 설명 단계
      - 각 플레이어에게 순서대로 발언권 부여 isSpeaking
      - 타이머 스레드 시작 (각 플레이어의 발언 시간 제한)
      - 플레이어의 설명을 받아(objectinputstream) 다른 모든 플레이어에게 브로드캐스트 (옵젝 스트림 계속 사용)

   b. 자유 토론 시간
      - 타이머 스레드 시작 (전체 토론 시간 제한)
      - 플레이어들의 메시지를 받아 모두에게 브로드캐스트

   c. 투표 단계
      - 투표 시작 신호 전송(클라 투표 화면으로 바꾸게)
      - 새 스레드. DataInputStream으로 각 플레이어의 투표를 받고 다 받으면 집계까지. 이후 투표 결과 계산 및 모든 플레이어에게 결과 전송 (기존 objstream)

   d. 최후변론 및 제거 결정
      - 지목된 플레이어에게 최후변론 기회 부여 (음성 데이터 처리 필요 > 어케 하는 건지 알아보기)
      - 다른 플레이어들의 최종 결정을 받아 처리 (투표 스레드 재사용)
      - 결과에 따라 게임 진행 또는 종료 처리

   e. 밤 단계 (라이어 추리 시간)
      - 라이어들에게만 메시지 전송 가능하도록 설정
      - 라이어의 추리 결과 접수 및 판정

5. 게임 종료 처리
   - 게임 결과를 모든 플레이어에게 전송
   - 플레이어들을 대기방으로 돌려보내기

추가 고려사항:

1. 타이머 스레드 구현 - 프론트에 이미 돼있긴 함
   ```java
   class GameTimer extends Thread {
       int remainingTime;
       // 타이머 로직 구현
   }
   ```

2. 메시지 처리
   - ChatMsg 클래스를 확장하여 게임의 다양한 상황을 처리할 수 있는 메시지 타입 정의

3. 동기화 처리
   - 여러 스레드가 동시에 접근하는 공유 자원(플레이어 목록, 게임 상태 등) 생각 잘 해야

4. 예외 처리
   - 클라이언트 연결 끊김, 네트워크 오류 등에 대한 견고한 예외 처리
   - 뭐 이건 수업때 배운대로

5. 로깅
   - 게임 진행 상황, 에러 등을 로그로 기록하여 디버깅 및 모니터링에 활용? 서버에 띄우면 좋을거같고

스레드 어떻게?? 게임상태관리, 클라통신, 채팅, 타이머 4개?