#Blue/Green 
배포는 두 개의 분리된 동일한 환경을 생성하는 배포 전략입니다. 한 환경(Blue)은 현재 애플리케이션 버전을 실행하고, 다른 환경(Green)은 새 애플리케이션 버전을 실행합니다. Blue/Green 배포 전략을 사용하면 배포가 실패할 경우 롤백 프로세스를 단순화하여 애플리케이션 가용성을 높이고 배포 위험을 줄입니다. Green 환경에 대한 테스트가 완료되면 실시간 애플리케이션 트래픽이 Green 환경으로 전송되고 Blue 환경은 더 이상 사용되지 않습니다.

A blue/green deployment is a deployment strategy in which you create two separate, but identical environments. 
blue environment is running the current application version and green environment is running the new application version. Using a blue/green deployment strategy increases application availability and reduces deployment risk by simplifying the rollback process if a deployment fails. 
Once testing has been completed on the green environment, live application traffic is directed to the green environment and the blue environment is deprecated.