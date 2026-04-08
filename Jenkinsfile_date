import java.text.SimpleDateFormat

// 빌드 시작 시간을 기반으로 날짜 생성
def TODAY = (new SimpleDateFormat("yyyyMMdd")).format(new Date())

pipeline {
    agent any

    environment {
        // BUILD_ID는 Jenkins 기본 제공 변수입니다.
        strDockerTag = "${TODAY}_${BUILD_ID}"
        strDockerImage = "studyuann/cicd-test:${strDockerTag}" 
    }

    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/studyuann/cicd-test.git'
            }
        }

        stage('Docker Image Build') {
            steps {
                script {
                    // 환경 변수에서 정의한 이미지명과 태그를 사용합니다.
                    oDockImage = docker.build("${env.strDockerImage}", "-f Dockerfile .")
                }
            }
        }

        stage('Docker Image Push') {
            steps {
                script {
                    docker.withRegistry('', 'docker-auth') {
                        oDockImage.push()
                        // (선택) latest 태그도 같이 push하고 싶다면:
                        // oDockImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-Privatekey']) {
                    // rm 명령어 실패(컨테이너 없음) 시 빌드 중단을 방지하기 위해 || true 추가
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.35.149.250 'docker container rm -f sampleweb || true'"
                    // 새 태그가 적용된 이미지를 실행
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.35.149.250 'docker run -d -p 80:80 --name sampleweb ${env.strDockerImage}'"
                }
            }
        }
    }
}