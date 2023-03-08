pipeline {
    agent { 
        node {
            label 'docker-tdp-builder'
            }
      }
    environment {
        number="${currentBuild.number}"
      }
    triggers {
        pollSCM '0 1 * * *'
      }
    stages {
        stage('clone') {
            steps {
                echo "Cloning..."
                git branch: 'branch-0.9.1-TDP-alliage', url: 'https://github.com/Yanis77240/tez'
            }
        }
        stage('Build') {
            steps {
                echo "Building..."
                sh '''
                mvn clean install -pl !tez-ui -Phadoop28 -P !hadoop27 -DskipTests
                '''
            }
        }
        /*stage('Test') {
            steps {
                echo "Testing..."
                sh '''
                mvn test -pl \!tez-ui -Phadoop28 -P\!hadoop27 --fail-never
                '''
            }
        }*/
        stage("Publish jars to Nexus") {
            steps {
                echo "Deploy..."
                withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh 'mvn deploy -pl !tez-ui -Phadoop28 -P !hadoop27 -DskipTests -s settings.xml'
                }
            }        
        }
        stage("Publish tar.gz to Nexus") {
            steps {
                echo "Publish tar.gz..."
                withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh 'curl -v -u $user:$pass --upload-file tez-dist/target/tez-0.9.1-TDP-0.1.0-SNAPSHOT.tar.gz http://172.19.0.2:8081/repository/maven-tar-files/tez/tez-0.9.1-TDP-0.1.0-SNAPSHOT-${number}.tar.gz'
                }
            }        
        }
    }
}