# farm_system_union(IoTogether) / 10월 3주차 기술보고서
##  - (산업시스템공학과 2022112386 김호준)


## 본격적인 아두이노 제어

  학교에서 아두이노 보드를 대여할 수 있다고 하여, 다행히 장비를 받고 본격적인 활동을 시작할 수 있게 되었다. 필자가 대여받은 기기는 무선 Wifi 모듈이 내장된 D1보드로 따로 무선 모듈을 추가할 필요가 없이 Wifi 기능을 활용할 수 있다.

<div align="center">
 <img width="230" height="219" src="https://github.com/user-attachments/assets/c72caf80-2907-42a0-9c7d-c35bb563defb" />
</div>

---

## 소프트웨어 셋업

그 전에 아두이노 자체를 제어하기 위한 소프트웨어 설정을 모두 마쳐야 한다.

아두이노는 USB를 통한 단순 데이터 통신이 아닌 시리얼 통신을 이용하기에 USB포트를 통한 시리얼 통신을 지원하기 위한 통칭 'CH340' 드라이버를 설치해야한다

PC와 아두이노 기기가 연결되면, 장치 관리자에서 새로운 포트(USB Serial Port)가 나타나면서 제대로 설치되었는지 여부를 판단할 수 있다.
<div align="center">
 <img width="2559" height="1439" src="https://github.com/user-attachments/assets/9b9026b0-2ca4-4308-a2ae-2922110d566d" />
</div>

IDE 내부에서도 설정할 것이 존재한다. 필자는 D1 아두이노 보드를 활용할 계획이기에 해당 보드에 대한 정보를 IDE 내부로 불러와 통신을 원활하게 할 수 있도록 한다.

새로 추가된 소프트웨어 포트에 맞게, 그리고 보유한 보드의 종류가 맞도록 설정을 변경하도록 한다.
<div align="center">
 <img width="500" src="https://github.com/user-attachments/assets/bbe31cb8-592d-4c2a-a8f9-2bc5d0cb9ec6" />
</div>

---
 
## 기기의 제어 확인
 
 기기의 상태의 확인할 겸, 초음파 센서를 활용한 거리 측정 코드를 작성하여 기기에 업로드 해보았다. 다행히 거리에 따른 출력이 정상 작동함을 확인해 볼 수 있었다.
<div align="center">
 <img width="400" src="https://github.com/user-attachments/assets/1491641d-51ca-400c-bfc9-5ddf5bf8414e" />
 <img width="400" src="https://github.com/user-attachments/assets/5ce34346-6088-4222-8517-e8808522a927" />

 <img width="2559" height="1439" src="https://github.com/user-attachments/assets/add42288-0b78-47f2-8d4f-ea6d57ac6181" />
</div>

 여기서 호기심으로 LCD 디스플레이를 활용하면서 한 번 응용보았다. 교재에 작성된 것을 토대로 새롭게 코드를 작성해보았다.

 하지만 LCD 디스플레이의 경우, I2C통신을 메인으로 하는 모듈이기도 하고, LCD를 위한 기본적인 명령문들이 초기적으로 없기 때문에, 각각을 보완해줄 수 있는 '라이브러리'를 설치해줘야 한다.

I2C통신을 위한 라이브러리는 자체적으로 내장되어 있어, #include로 바로 불러올 수 있지만,

LCD 디스플레이 라이브러리는 IDE 자체에 포함된 '라이브러리 매니저'라는 창에서 따로 라이브러리를 설치하고 불러와야 한다.

<div align="center">
 <img width="300" src="https://github.com/user-attachments/assets/22f4c7b7-0ab9-487b-aac3-dc5dc2819fcc" />
 <img width="650" src="https://github.com/user-attachments/assets/22ca61ec-681d-45fe-affa-0b2cf09bce00" />
</div>

다행히 이번에도 정상작동 함을 볼 수 있었다. 참고로 소수점 아래에 5자리가 출력된 것은, 코드가 반복될 때마다 LCD디스플레이를 초기화를 안하고 넘어가서이기 때문이다.

<div align="center">
  <img width="2000" src="https://github.com/user-attachments/assets/a394633f-3f2b-46c8-8e34-7909d817cf91" />
</div>

---

## 텔레그램 연동

#### 텔레그램 설정

저번주 조별 회의에서 '텔레그램'을 이용한 모바일 알림 시스템을 구축하기로 하였으므로, 텔레그램을 해당 기능을 구현해보고자 한다.

우선 텔레그램에서 사용자를 위한 봇(bot)을 생성해야한다. "/start","/newbot","봇의 별명","봇의 이름"을 순서대로 입력하고,

필자에게 부여된 토큰을 따라 Chat_ID를 파악하였다.

<div align="center">
 <img width="400" src="https://github.com/user-attachments/assets/750dd8af-d88f-4b50-bc77-5142e504dc58" />
 <img width="400" src="https://github.com/user-attachments/assets/aa4c7347-d9a5-45ad-b1bf-bab0d6480a43" />
 <img width="1919" height="1199" src="https://github.com/user-attachments/assets/5d451199-3a9d-442a-b2d5-f907c845ca3b" />
</div>

#### IDE 설정

IDE에서는 텔레그램 연동을 위한 라이브러리인 "UniviersalTelegramBot.h"와 WiFi 설정을 만질 수 있는 관련 라이브러리들을 추가해줘야한다.

텔레그램 연동용 라이브러리의 경우, 이전의 LCD디스플레이 라이브러리처럼 라이브러리 매니저에서 직접 추가할 수 있다.

해당 과정은 조원들과 3차 회의이자 시스템 이해를 위한 테스트로 코드를 구현해 보았다. 참고로 위에서 작성하였던 초음파 센서와 연계해보았다.

코드는 다음과 같다.

```

#include <UniversalTelegramBot.h>
#include <WiFiClientSecure.h>
#include <ESP8266WiFi.h>

#define WIFI_SSID ""       //와이파이 이름 입력 "WIFI_NAME"
#define WIFI_PASSWORD ""   //와이파이 비밀번호 "123456"

#define BOT_TOKEN "8140194638:AAGOpEGxQ9Qf3nxU6sLhEFPsJLk23sPn7Jw"  // bot token(숫자와 영문의 나열)
#define CHAT_ID "5561347315"     // Chat_ID("숫자")

#define TRIG_PIN D7
#define ECHO_PIN D6

WiFiClientSecure client;
UniversalTelegramBot bot(BOT_TOKEN, client);

void setup() {
  Serial.begin(115200);

  Serial.print("Connecting to AP");
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  while (WiFi.status() != WL_CONNECTED){
    Serial.print(".");
    delay(200);
  }

  Serial.println("");
  Serial.println("WiFi connected.");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
  Serial.println();

  configTime(0, 0, "pool.ntp.org");

  Serial.print("Waiting for time synchronization");

  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
}

void loop() {
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  unsigned long duration = pulseIn(ECHO_PIN, HIGH);
  float distanceCM = ((34000.0 * (float)duration) / 1000000.0) / 2.0;
  Serial.println(distanceCM);

  if (distanceCM > 2 && distanceCM < 20){
    Serial.println("물체 감지!!");

    time_t now = time(nullptr);
    while (now < 24 + 3600){
      delay(100);
      Serial.print(".");
      now = time(nullptr);
    }
    Serial.println(" TIME SYNCHRONIZED. ");

    X509List cert(TELEGRAM_CERTIFICATE_ROOT);
    client.setTrustAnchors(&cert);

    bot.sendMessage(CHAT_ID, "물체가 감지되었습니다!!", "");
    for (int i = 0; i < 10; i++){
      delay(1000);
    }
  }
  delay(100);
}

```

먼저 아두이노 기기가 WiFi에 먼저 연결되고, 루프문이 본격적으로 실행되는 구조다.

이후, 초음파센서에서의 거리가 20cm 아래로 가까워질 때, 문자를 보내는 시스템이다. 

다행히 필자의 세팅에서는 아두이노와 텔레그램이 서로 잘 연동되어 문자를 정해진 조건 下에서 수신받을 수 있었다.

이로써 필자도 또한 무선 통신을 무사히(?) 구현할 수 있게 되었다.

<div align="center">
 <img width="400" src="https://github.com/user-attachments/assets/f30222fd-0611-4592-b961-5c36e8fda48c" />
</div>

---

## 어려웠던 점

기기 자체 또는 IDE 문제인지, 새롭게 코드를 작성하고 아두이노에 업로드를 하면, 이전에 설정한 코드를 토대로 아두이노가 작동하는 모습을 계속 볼 수 있었다.

인터넷을 통해 아두이노의 리셋 버튼을 누르면서 업로드를 하면 코드를 바꿀 수 있음을 알아냈지만 이마저도 실패의 경우가 더 많았었다.

본격적인 실습을 진행할 때에 해당 문제를 해결하느라 조원들에게 제대로 따라갈 수 있을지 또는 시간이 부족하지 않을지 불안감이 좀 있다.

그래도 성공적으로 코드가 업로드되면, 그 역할을 버그 없이 수행한다는 점에서 다행이라고 느끼는 부분이다.
