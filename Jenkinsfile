pipeline {
    agent any

    stages {
        stage('Build Spring Boot Project') {
            steps {
                script {
                    // Spring Boot 프로젝트 빌드
                    sh './gradlew clean build'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Docker 이미지 빌드
                    docker.build("jiwonlee42/spring-boot:1.0", "--file Dockerfile .")
                }
            }
        }

        stage('Stop and Remove Existing Container') {
            steps {
                script {
                    // 기존에 동작 중인 컨테이너 중지 및 삭제
                    sh 'docker ps -q --filter "name=spring-boot-server" | grep -q . && docker stop spring-boot-server && docker rm spring-boot-server || true'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Docker 컨테이너 실행
                    sh 'docker run -p 8081:8080 -d --name=spring-boot-server jiwonlee42/spring-boot:1.0'
                }
            }
        }

        stage('Clean Up Unused Docker Images') {
            steps {
                script {
                    // 태그가 겹친 이미지 삭제
                    sh 'docker rmi -f $(docker images -f "dangling=true" -q) || true'
                }
            }
        }
    }
}