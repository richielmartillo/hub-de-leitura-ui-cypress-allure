pipeline {
    agent any

    tools {
        nodejs 'nodejs'
    }

    environment {
        APP_DIR = 'hub-de-leitura-integrado2'
        TEST_DIR = 'hub-de-leitura-teste-ui'
        APP_REPOSITORY = 'https://github.com/EBAC-QE/hub-de-leitura-integrado2.git'
        TEST_REPOSITORY = 'https://github.com/richielmartillo/hub-de-leitura-teste-ui.git'
    }

    stages {
        stage('Clonar e iniciar a aplicacao') {
            steps {
                sh '''
                    rm -rf "$APP_DIR"
                    git clone --depth 1 "$APP_REPOSITORY" "$APP_DIR"
                    cd "$APP_DIR"
                    npm ci
                    nohup npm start > app.log 2>&1 &
                    echo $! > app.pid
                '''
            }
        }

        stage('Clonar e instalar o projeto de teste') {
            steps {
                sh '''
                    rm -rf "$TEST_DIR"
                    git clone --depth 1 "$TEST_REPOSITORY" "$TEST_DIR"
                    cd "$TEST_DIR"
                    npm ci
                '''
            }
        }

        stage('Esperar a aplicacao subir') {
            steps {
                dir(env.TEST_DIR) {
                    sh 'npx wait-on http://localhost:3000'
                }
            }
        }

        stage('Executar testes no Cypress Cloud') {
            steps {
                dir(env.TEST_DIR) {
                    withCredentials([string(credentialsId: 'cypress-record-key', variable: 'CYPRESS_RECORD_KEY')]) {
                        sh '''
                            npx cypress run \
                              --record \
                              --key "$CYPRESS_RECORD_KEY" \
                              --browser chrome \
                              --ci-build-id "jenkins-$BUILD_NUMBER" \
                              --group 'UI-Linux'
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            dir(env.TEST_DIR) {
                archiveArtifacts artifacts: 'cypress/screenshots/**/*.*,cypress/videos/**/*.*', allowEmptyArchive: true
                allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
            }
            dir(env.APP_DIR) {
                sh 'test ! -f app.pid || kill "$(cat app.pid)" || true'
            }
        }
    }
}
