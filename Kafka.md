#Kafka
# 오픈소스 메시징 시스템

Message Queue - 분산화된 환경에서 발신자와 수신자 사이에서 메시지를 전송, 수신하는 기술.
MOM (Message Oriented Middleware)을 통해서 구현된다.

왜 사용하나?
- **느슨한 결합** (발신자와 수신자가 서로 의존 X)
- **보장성** (발신자가 발생한 모든 메시지는 수신자에게 전달된다.)

구분
- **Point to Point** (P2P) : 한 대의 발신자가 한 대의 수신자에게 메시지를 보낸다. 
- **Pub/Sub** : 발신자가 메시지를 전송한다. (토픽) 해당 공간을 구독하는 수신자 모두 메시지를 수신한다. 

RabbitMQ & ActiveMQ는 모두 지원하나, kafka는 pub/sub 모델만을 지원한다.

**모든 흐름이 카프카로 모인다.**



[what-is-kafka](https://hudi.blog/what-is-kafka/)
https://fastcampus.co.kr/story_article_newkafka