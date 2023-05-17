pipeline {

    agent any
/*
	tools {
        maven "maven3"
    }
*/
    environment {
        registry = "isrealurephu/metro-app"
        registryCredential = "dockerhub"
    }

    stages{


        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

        stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }

        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }

        stage('CODE ANALYSIS with SONARQUBE') {

            environment {
                scannerHome = tool 'mysonarscanner4'
            }

            steps {
                withSonarQubeEnv('sonar-pro') {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }

                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage ( "Build Docker App Images" ) {
           steps {
             script {
             dockerImage = docker.build registry + ":V$BUILD_NUMBER"
             }
           }

        }

        stage ( "Upload Images to dockerhub") {
          steps {
            script {
              docker.withRegistry ( '', registryCredential) {
                dockerImage.push ("V$BUILD_NUMBER")
                dockerImage.push ("latest")
              }
            }
          }
        }

        stage ('Remove unused docker images') {
          steps {
            sh "docker rmi $registry:V$BUILD_NUMBER"
          }
        }



        stage('deploy to lke cluster') {
            steps {
                script {
                   echo 'deploying docker image...'
                   withKubeConfig([credentialsId: "metro-lke", serverUrl: 'https://b0136ffc-adcd-4984-8ff0-f9dd24d495e1.eu-west-1.linodelke.net']){
                       sh 'helm upgrade --install --force vprofile-stack  helm/vprofilecharts --set appimage=${registry}:V${BUILD_NUMBER} --set namespace=prod --namespace prod'

                   }
                
                }
            }

    }


}
}


