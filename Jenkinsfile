pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Checking out SCM...'
                checkout scm
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                echo 'Deploying with Docker Compose...'
                sh '''
                    # 새로운 이미지를 빌드하고 모든 컨테이너 실행
                    docker-compose up -d --build
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline completed successfully.'
            script {
              // Git 커밋 정보
              def Author_ID = sh(script: "git show -s --pretty=%an", returnStdout: true).trim()
              def Author_Name = sh(script: "git show -s --pretty=%ae", returnStdout: true).trim()

              mattermostSend(
                color: 'good',
                message: "빌드 성공: ${env.JOB_NAME} #${env.BUILD_NUMBER} by ${Author_ID}(${Author_Name})\n(<${env.BUILD_URL}|Details>)", 
                endpoint: 'https://meeting.ssafy.com/hooks/jf7jzm17zjrtzp8qzzd43mbxje', 
                channel: 'drmma_jenkins'
              )
            }
        }

        failure {
            echo 'Pipeline failed.'
            script {
              // Git 커밋 정보를 가져오는 부분
              def Author_ID = sh(script: "git show -s --pretty=%an", returnStdout: true).trim()
              def Author_Name = sh(script: "git show -s --pretty=%ae", returnStdout: true).trim()

              mattermostSend(
                color: 'danger', 
                message: "빌드 실패: ${env.JOB_NAME} #${env.BUILD_NUMBER} by ${Author_ID}(${Author_Name})\n(<${env.BUILD_URL}|Details>)", 
                endpoint: 'https://meeting.ssafy.com/hooks/jf7jzm17zjrtzp8qzzd43mbxje', 
                channel: 'drmma_jenkins'
              )
            }
        }
    } 
}
