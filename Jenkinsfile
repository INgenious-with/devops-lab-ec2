pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-lab-ec2"
        IMAGE_TAG  = "latest"
        CONTAINER_NAME = "my-nginx"
        PORT = "8081"
    }

    stages {
        stage('Git Checkout') {
            steps {
                echo "🔹 GitHub에서 코드 가져오기"
                git branch: 'main', url: 'https://github.com/INgenious-with/devops-lab-ec2.git'
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

        stage('Stop and Remove Old Container') {
            steps {
                echo "🔹 기존 Docker 컨테이너 중지 및 삭제"
                script {
                    try {
                        // 기존 컨테이너가 실행 중이면 강제로 종료하고 삭제
                        sh '''
                            # 컨테이너가 실행 중인지 확인
                            CONTAINER_ID=$(docker ps -q -f name=$CONTAINER_NAME)
                            if [ ! -z "$CONTAINER_ID" ]; then
                                echo "기존 컨테이너가 존재합니다. 중지 및 삭제합니다."
                                docker stop $CONTAINER_NAME || true  # 실행 중인 컨테이너 종료
                                docker rm $CONTAINER_NAME || true    # 컨테이너 삭제
                                docker kill $CONTAINER_NAME || true  # 강제로 종료
                            else
                                echo "기존 컨테이너가 없습니다."
                            fi
                        '''
                        echo "✅ 기존 컨테이너 중지 및 삭제 완료"
                    } catch (err) {
                        echo "❌ 기존 컨테이너 중지 및 삭제 실패"
                        error("Failed to stop and remove old container")
                    }
                }
            }
        }

        stage('Run New Container') {
            steps {
                echo "🔹 새 Docker 컨테이너 실행"
                script {
                    try {
                        sh 'docker run -d --name $CONTAINER_NAME -p $PORT:80 $IMAGE_NAME:$IMAGE_TAG'
                        echo "✅ 새 Docker 컨테이너 실행 성공"
                    } catch (err) {
                        echo "❌ 새 Docker 컨테이너 실행 실패"
                        error("Failed to run new container")
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