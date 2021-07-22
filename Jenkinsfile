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
                            bat 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                bat 'mvn clean package sonar:sonar'
                            }

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
                            bat 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                bat 'mvn clean package sonar:sonar'
                            }

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
                            bat 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                bat 'mvn clean package sonar:sonar'
                            }

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
                            bat 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                bat 'mvn clean package sonar:sonar'
                            }

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
                            bat 'mvn test'
                            withSonarQubeEnv('SonarQube') {
                                bat 'mvn clean package sonar:sonar'
                            }

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

        stage('quality gate analysis') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
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
