pipeline {
    agent {
        docker { image 'debian'
                    args '-u root'
            }
    }
    stages {
        stage('Clone') {
            steps {
                git branch:'master', url:'https://github.com/GitHubeMail1ASIR/ic-diccionario.git'
            }
        }
        stage('Install') {
            steps {
                sh 'apt update && apt install -y aspell-es'
            }
        }
        stage('Test') {
            steps {
                sh '''
                export LC_ALL=C.UTF-8
                OUTPUT=`cat doc/*.md | aspell list -d es -p ./.aspell.es.pws`; if [ -n "$OUTPUT" ]; then echo $OUTPUT; exit 1; fi'''
            }
        }
    }
    post {
        always {
            mail to: 'jjas-asir2@Debian',
            subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
            body: "${env.BUILD_URL} has result ${currentBuild.result}"
        }
    }
}
