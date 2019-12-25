pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "eureka:Marko"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/MarkoMitrovicRAF/raf.git'
            }
        }
        stage ('Docker') {
            when {
                branch 'master'
            }
            steps{
                script{
                    sh '''
                            docker image build -t eureka:marko .
                        '''
                  // def app=docker.build(DOCKER_IMAGE_NAME)
                }
            }
            post {
                failure {
                    emailext (
                        subject: "Job '${env.JOB_NAME} ${env.BUILD_NUMBER} ",
                        body: """CI/CD pipeline greska u "Docker" fazi. Log fajl se moze videti na: href=${env.BUILD_URL} """,
                        to: "mmitrovic@raf.rs",
                        from: "jenkins@jenkins.netcast.rs"
                    )
                }
            }
        }
        stage ('Deploy') {
            when {
                branch 'master'
            }
            steps{
                script{
                    sh '''
                            docker run -d --name eureka -p 24000:8761 eureka:latest
                        '''
                }
            }
            post {
                failure {
                    emailext (
                        subject: "Job '${env.JOB_NAME} ${env.BUILD_NUMBER} ",
                        body: """CI/CD pipeline greska u "Build" fazi. Log fajl se moze videti na: href=${env.BUILD_URL} """,
                        to: "mmitrovic@raf.rs",
                        from: "jenkins@jenkins.netcast.rs"
                    )
                }
            }
        }
    }
}

