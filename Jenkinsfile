pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'Java'
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

                            dockerImage = docker.build registry + "/api-gateway:$BUILD_NUMBER"

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
                            dockerImage = docker.build registry + "/eureka:$BUILD_NUMBER"
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
                        }}
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
                        }}
                    } catch (error) {
                        throw error
                        }
                    }
                }
            }
        stage('checking monorepo caard-service') {
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
                        }}
                    } catch (error) {
                        throw error
                        }
                    }
                }
            }

            stage('showing code coverage') {
                steps {
                step([$class: 'JacocoPublisher',
      execPattern: 'target/*.exec',
      classPattern: 'target/classes',
      sourcePattern: 'src/main/java',
      exclusionPattern: 'src/test*'])}
            }

/*
            stage('Running Tests') {
            steps {
                script {
                    try {
                        sh 'mvn test'
                    }
                   catch (error) {
                        throw error
                   }
                }
            }
            }

       stage('Building Project'){
        steps{
            script{
                 sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install"
            }
        }
       }

        stage('Analysing Coverage') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }

            post {
                success {
                    step([$class: 'JacocoPublisher',
      execPattern: 'target/*.exec',
      classPattern: 'target/classes',
      sourcePattern: 'src/main/java',
      exclusionPattern: 'src/test*'])
                }
            }
        }
          */

/*
        stage("Quality gate Analysis") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }

        stage("Dockerising Images")
        {
            steps{
                 script{

                        sh "docker login -u akshit2707 -p password123"
                 }
            }
        }

        stage("Pushing to DockerHub")
        {
            steps{
                 script{

                          sh 'docker-compose up  --no-start'
                          sh 'docker-compose push'
                 }
            }
        }
        */
        }
        }
