.
├── global-var/
│   └── main.yml           # 전역 변수 (Region, Prefix, CIDR 등)
├── site.yml               # 메인 실행 플레이북
└── roles/
    ├── iam/               # IAM Policy, Role, Instance Profile
    ├── vpc/               # VPC, Subnet, IGW, NAT, Routing
    ├── security/          # Security Groups (SSH, HTTP, HTTPS)
    ├── network_alb/       # ALB, Target Groups, Route 53
    ├── jenkins/           # Jenkins EC2 및 초기 스크립트 실행
    └── app_asg/           # AMI, Launch Template, Auto Scaling

3. Ansible 실행 명령어
> ansible-playbook site.yml -e "infra_state=present"

4. 인프라 전체 생성 (Deploy)
의존성 순서에 따라 IAM부터 ASG까지 한 번에 생성합니다.   
> ansible-playbook site.yml -e "infra_state=present"   

5. 인프라 전체 삭제 (Terminate)   
생성의 역순으로 리소스를 안전하게 제거하며, NAT에 할당된 EIP도 함께 반납합니다.   
> ansible-playbook site.yml -e "infra_state=absent"   

6. Role별 개별 제어 (태그 활용)   
특정 부분만 수정하거나 다시 배포할 때 사용합니다.   

> VPC만 생성   
> ansible-playbook site.yml --tags "vpc" -e "infra_state=present"

> 로드밸런서와 DNS 레코드만 삭제
> ansible-playbook site.yml --tags "alb" -e "infra_state=absent"    