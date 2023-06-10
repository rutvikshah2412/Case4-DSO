pipeline {
    agent any
    tools{
        jdk 'jdk11'
        maven 'case2-maven'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rutvikshah2412/Case2-DSO.git'
            }
        }
//         stage('Compile') {
//             steps {
//                sh "mvn clean compile"
//             }
//         }
        stage('Build') {
            steps {
               sh "mvn clean package -DskipTests=true"
            }
        }
        stage('Download Unified Agent') { steps {
            sh 'curl -LJO https://unified-agent.s3.amazonaws.com/wss-unified-agent.jar'
        }}
        stage('Run Unified Agent') { steps {
            sh 'java -jar wss-unified-agent.jar -detect -d .'
        }}
        stage('Initiate Offline Scan'){steps {
            sh 'java -jar wss-unified-agent.jar -c wss-generated-file.config -d . -apiKey ac1e3008979c440db1c44029318ef9eba6265539d11c4b02b34c2eb74df79a96 -userKey a3ac691cf342494c9e42b435876acae0fc3bb9bc6da5454c8684c77e7bc65970 -product case2DSO -project Case2DSO -offline true'
        }}
        stage('Upload Reports'){steps {
             sh 'java -jar wss-unified-agent.jar -c wss-generated-file.config -requestFiles whitesource/update-request.txt'
        }}
    }
}
