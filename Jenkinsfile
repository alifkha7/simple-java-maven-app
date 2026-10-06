node {
    checkout scm

    stage('Build') {
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            sh 'mvn -B -DskipTests clean package'
        }
    }

    stage('Test') {
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            try {
                sh 'mvn test'
            } finally {
                junit 'target/surefire-reports/*.xml'
            }
        }
    }

    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
    }

    stage('Deploy') {
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            sh './jenkins/scripts/deliver.sh'
            sleep 60
        }
    }
}
