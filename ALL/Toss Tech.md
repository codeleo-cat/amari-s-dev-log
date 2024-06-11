[# 서버 증설 없이 처리하는 대규모 트래픽](https://toss.tech/article/monitoring-traffic)

**Throttling (스로틀링)**
기기의 CPU 등이 지나치게 과열될 때 기기의 손상을 막고자 클럭과 전압을 강제적으로 낮추거나 전원을 꺼서 발열을 줄이는 기능
-> 성능을 강제로 낮춤.


Redis에서 지연이 발생 > Redis를 사용하는 모든 곳에서 지연 발생 > 
해결 - 웹 서버에서 local cache를 사용해서 data를 서버 내에서 전부 캐싱하는 방법
데이터를 압축하면 크키가 작아져서 메모리 사용량 