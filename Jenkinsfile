pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-lab-ec2"
        IMAGE_TAG  = "latest"
    }

    stages {
        stage('Git Checkout') {
            steps {
                echo "🔹 GitHub에서 코드 가져오기"
                git branch: 'main', url: 'git@github.com:INgenious-with/devops-lab-ec2.git'
                echo "✅ Git Checkout 완료"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🔹 Docker 이미지 빌드 시작"
                script {
                    try {
                        sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
                        echo "✅ Docker 이미지 빌드 성공"
                    } catch (err) {
                        echo "❌ Docker 이미지 빌드 실패"
                        error("Build failed")
                    }
                }
            }
        }

        stage('Run Container (Test)') {
            steps {
                echo "🔹 Docker 컨테이너 실행 및 테스트"
                script {
                    try {
                        sh 'docker run --rm $IMAGE_NAME:$IMAGE_TAG echo "컨테이너 테스트 성공!"'
                        echo "✅ Docker 컨테이너 테스트 성공"
                    } catch (err) {
                        echo "❌ Docker 컨테이너 테스트 실패"
                        error("Test failed")
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                echo "🔹 Docker 이미지 정리"
                sh 'docker rmi $IMAGE_NAME:$IMAGE_TAG || true'
                echo "✅ Clean up 완료"
            }
        }
    }

    post {
        success {
            echo "🎉 전체 빌드 성공!"
        }
        failure {
            echo "⚠️ 전체 빌드 실패!"
        }
    }
}
