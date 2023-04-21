podTemplate(containers: [
    containerTemplate(
        name: 'tdp-builder', 
        image: 'yanisbariteau/tdp-builder:jenkins', 
        command: 'sleep', 
        args: '30d'
        )
  ]) {

    node(POD_LABEL) {
        container('tdp-builder') {
            environment {
                number="${currentBuild.number}"
            }
            stage('Git Clone') {
                echo "Cloning..."
                git branch: 'branch-0.9.1-TDP-alliage-k8s', url: 'https://github.com/Yanis77240/tez'
            }   
            stage ('Build') {
                echo "Building..."
                sh '''
                mvn clean install -pl !tez-ui -Phadoop28 -P!hadoop27 -DskipTests
                '''
            }
            /*stage('Test') {
                echo "Testing..."
                sh '''
                mvn test -pl \!tez-ui -Phadoop28 -P\!hadoop27 --fail-never
                '''
            }*/
            stage('Deliver') {
                echo "Deploy..."
                withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh 'mvn clean deploy -pl !tez-ui -Phadoop28 -P !hadoop27 -DskipTests -s settings.xml'
                }
            }
            stage("Publish tar.gz to Nexus") {
                echo "Publish tar.gz..."
                withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh 'curl -v -u $user:$pass --upload-file tez-dist/target/tez-0.9.1-TDP-0.1.0-SNAPSHOT.tar.gz http://10.110.4.212:8081/repository/maven-tar-files/tez/tez-0.9.1-TDP-0.1.0-SNAPSHOT-${number}.tar.gz'
                }
            }       
        }
    }
}
