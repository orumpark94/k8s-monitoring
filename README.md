
# Prometheus + Grafana를 이용한 k8s 클러스터 모니터링 시스템 구축

(Gitaction / Ansible / Helm chart / Prometheus / Grafana 사용)

# 구성 목표
 
GitAction / Ansible 을 사용 Helm을 이용하여 Prometheus,Grafana을 구축하고 이를 통하여 k8s 내에 있는 node들에 대한 자원들에 대해 모니터링 시스템을 구축하는 것을 목적으로 한다.

# 구성 순서도

1.Github Actions 구성

  .github/workflows/ deploy파일 작성
  inventory.ini에서 대상 서버의 Ansible 인벤토리 설정
  GitHub Actions 실행 시 Self-Hosted Runner를 이용하여 가상 서버와 연결
  Ansible 플레이북 구성

2.Ansible Playbook 구성

    .github/workflows/deploy.yaml  #gitaciton을 통한 배포
    helm_install.yaml   #helm chart를 이용한 서비스 배포
    inventory.ini   #대상 서버 지정
    prometheus_grafana.yaml  #배포 서비스 설정
    
playbook 구성하여 실행

![image](https://github.com/user-attachments/assets/d8428964-c234-4080-aea4-8484b8a34880)

배포완료

3.Prometheus 및 Grafana 서비스 확인

![image](https://github.com/user-attachments/assets/2e1b3607-b498-434a-8d48-178383823388)

k8s에서 사용된 CNI 는 flannel을 사용하였습니다.

k8s cluster의 구성은 ( 1 master node 2 worker node 구성이며, 연결은 SSH를 이용하여 연결하였습니다 )
 
각 서비스(Prometheus / Grafana) 서비스 확인

![image](https://github.com/user-attachments/assets/e101260a-9565-4244-af07-67c84d23e905)

![image](https://github.com/user-attachments/assets/c624449e-3cec-4591-9740-55e68142a1e6)


4.결과

Prometheus 쿼리 실행 및 Grafana 노드 리소스 확인 (CPU 리소스 확인)

![image](https://github.com/user-attachments/assets/47f2138e-33b0-44df-b421-37308343c60e)

![image](https://github.com/user-attachments/assets/acdee636-13f7-4d32-87ff-e316995de48c)


# 주의사항 및 구성시 애로사항

1.Helm chart를 이용해서 배포를 진행한다면, 배포의 난이도는 낮아지나 기본적으로 chart를 기반으로 배포되기에 커스터마이징이 어렵다.
helm update 및 configmap 적용 불가 → 해결방안: helm chart로 배포시에 원하는 커스터마이징 설정을 넣어 놓고 배포를 시작해야합니다.

2.Helm chart로 배포시 kubectl로 pod 및 cluster에 대한 설정 변경은 적절하지 않습니다.


