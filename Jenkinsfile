def TODAY = (new SimpleDateFormat("yyyyMMdd")).format(new Date())

pipeline {
    agent any

    environment{
        strDockerTag="${TODAY}_${BUILD_ID}"
10      strDockerImage="sunnykid7/cicd-test:0.1"
    }
    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/studyuann/cicd-test.git'
            }
        }

        stage('Docker Image Build') {
            steps {
                script{
                    oDockImage = docker.build(strDockerImage,"-f Dockerfile .")
                }
            }
        }
        stage('Docker Image Push'){ 
            // 오타 주의: Dokcer -> Docker
            steps {
                script {
                    docker.withRegistry('', 'docker-auth'){
                        oDockImage.push()
                    }
                }
            }
        }
      stage('Deploy Server') {
    steps {
        sshagent(credentials: ['Deploy-Privatekey']) {
            // 1. 우선 권한이 있는 홈 디렉토리로 파일을 보냅니다.
            sh "ssh -o StrictHostKeyChecking=no ubuntu@3.35.149.250  docker container rm -f sampleweb "
            // 2. SSH로 접속하여 sudo 권한으로 파일을 웹 서버 경로로 복사합니다.
            // 주의: 서버의 ubuntu 계정이 'sudoers'에서 NOPASSWD 설정이 되어 있어야 합니다.
            sh "ssh -o StrictHostKeyChecking=no ubuntu@3.35.149.250  docker run -d -p 80:80 --name sampleweb ${strDockerImage}"
                 }
            }
        }
    }
}