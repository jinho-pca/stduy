# CI/CD Jenkins

## 왜 CI/CD 인가?

- 워터폴 프로세스는 딜리버리가 One time
- 애자일 프로세스는 Many times스프린트마다 딜리버리 요구사항 분석 -> 설계 -> 개발 ->QA일의 양이 많다.릴리즈 할때 자동화를 해서 제일 부담을 줄이자.

## Jenkins 소개

- Java로 작성된 자동화 서버
- 다양한 플러그인을 사용하여 CI/CD 파이프라인을 만들어 자동화 작업을 완성
- MIT License

### 베어 인스톨

- 인스톨러를 사용해 PC, 서버 설치
- JRE 8, 11 만 지원

### 컨테이너

```
docker pull jenkins/jenkins
```

- 컨테이너에 JRE가 포함
- docker 컨테이너에 대한 이해 필요

## Live 사연

1. docker-hub => jenkins/jenkins

### Docker file

Dockerfile

1 2 3 4 5 6 7 8

```
FROM jenkins/jenkins COPY ~~.pem /var/jenkins_home USER root RUN chmod 600 /var/jenkins_home/~~.pem RUN apt-get update RUN apt-get install -y python RUN apt-get install -y npm 
```

### shell

```
docker build -t jenkins. docker run -d --name jenkins -p 8080:8080 -p 50000:50000 jenkins:1.0
```

### Jenkins 관리 - Credentials

Jenkins store -> add credentials

username: email

pasword: pwd

id: ~~

### 새로운 Item (pipe line) - Script

- 파이프라인을 쓰면 중간 과정을 볼 수 있다.

```
pipeline{    agent any        stages{        stage("prepare"){            steps{                git credentialsId: "ssafy", url: "~~"                sh "npm install"            }        }                stage("build"){            steps{            sh "npm run build"                        }            post{                success{                    sh "scp -i /var/jenkins_home/~~.pem -q -o StrictHostKeyChecking=no -r ./dist/ ubuntu@~~~.io:/home.ubuntu/test"                    echo "Success"                }            }        }    } }
```

### 간단하게 확인

```
sudo python3 -m http-.server 8000
```

### 시나리오

- gitlab -> pull into container

- npm install => npm run build

- copy dist ./ ec2

  