pipeline {
    agent any

    tools {
        maven 'maven'
    //jdk 'Java'
    }

    environment  {
        dockerImage = ''
        registry = 'akshit2707'

        //provide credentials in jenkins credentials and tag it as docker_id
        registryCredential = 'docker_id'
    }

    stages {
        stage('Cloning Repository') {
            steps {
                git branch: 'master' , url: 'https://github.com/aarsh2211/microservices-2.git'
            }
        }

        stage('checking monorepo api-gateway') {
            when {
                changeset '**/api-gateway/*.*'
            }
            steps {
                echo 'Building for api-gateway'

                script {
                    try {
                        dir('api-gateway') {
                                sh 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                sh 'mvn clean package sonar:sonar'
                            }

                            waitForQualityGate abortPipeline: true
                            dockerImage = docker.build registry + '/api-gateway:latest'

                            docker.withRegistry( '', registryCredential ) {
                                script
                               {
                                    dockerImage.push() }
                            }
                        }
                    } catch (error) {
                        throw error
                    }
                }
            }
        }

        stage('checking monorepo eureka') {
            when {
                changeset '**/eureka/*.*'
            }
            steps {
                echo 'Building for eureka'
                script {
                    try {
                        dir('eureka') {
                            sh 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                sh 'mvn clean package sonar:sonar'
                            }
                            waitForQualityGate abortPipeline: true
                            dockerImage = docker.build registry + '/eureka:latest'
                            docker.withRegistry( '', registryCredential )
                            {
                                dockerImage.push()
                            }
                        }
                    } catch (error) {
                        throw error
                    }
                }
            }
        }

        stage('checking monorepo product-service') {
            when {
                changeset '**/product-service/*.*'
            }
            steps {
                echo 'Building for product-service'
                script {
                    try {
                        dir('product-service') {
                            sh 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                sh 'mvn clean package sonar:sonar'
                            }
                            waitForQualityGate abortPipeline: true
                            dockerImage = docker.build registry + '/product-service:latest'

                            docker.withRegistry( '', registryCredential ) {
                                script
                               {
                                    dockerImage.push() }
                            }
                        }
                    } catch (error) {
                        throw error
                    }
                }
            }
        }

        stage('checking monorepo user-service') {
            when {
                changeset '**/user-service/*.*'
            }
            steps {
                echo 'Building for user-service'
                script {
                    try {
                        dir('user-service') {
                            sh 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                sh 'mvn clean package sonar:sonar'
                            }
                            waitForQualityGate abortPipeline: true

                            dockerImage = docker.build registry + '/user-service:latest'

                            docker.withRegistry( '', registryCredential ) {
                                script
                               {
                                    dockerImage.push() }
                            }
                        }
                    } catch (error) {
                        throw error
                    }
                }
            }
        }
        stage('checking monorepo card-service') {
            when {
                changeset '**/card-service/*.*'
            }
            steps {
                echo 'Building for card-service'
                script {
                    try {
                        dir('card-service') {
                            sh 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                sh 'mvn clean package sonar:sonar'
                            }
                            waitForQualityGate abortPipeline: true
                            dockerImage = docker.build registry + '/card-service:latest'

                            docker.withRegistry( '', registryCredential ) {
                                script
                               {
                                    dockerImage.push() }
                            }
                        }
                    } catch (error) {
                        throw error
                    }
                }
            }
        }

            stage('showing code coverage') {
                steps {
                step([$class: 'JacocoPublisher',
      execPattern: '**/target/*.exec',
      classPattern: '**/target/classes',
      sourcePattern: 'src/main/java',
      exclusionPattern: 'src/test*'])}
            }
    }
}
