node {
    checkout scm
    // Automation Build
    docker.image('maven:3.9.2-eclipse-temurin-11').inside('-v /root/.m2:/root/.m2') {
        stage('Build') {
            sh 'mvn --version'                
            sh 'mvn -B -DskipTests clean package'
        }
        stage('Test') {            
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
        stage('Manual Approval') {            
            input(message:'Lanjutkan ke tahap Deploy?', ok:'Proceed')
            // junit 'target/surefire-reports/*.xml'
            // Proceed (melanjutkan eksekusi pipeline ke tahap Deploy)
            // Abort (menghentikan eksekusi pipeline)
        }
        stage('Deploy') {       
            sh 'mvn test'
            sh './jenkins/scripts/deliver.sh'
            sleep time: 1, unit: 'MINUTES'
        }   
    }
}
