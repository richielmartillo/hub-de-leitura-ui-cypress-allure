pipeline {
    agent any

    tools {
        nodejs 'nodejs'
    }

    environment {
        APP_DIR = 'hub-de-leitura-integrado2'
        TEST_DIR = 'hub-de-leitura-ui-cypress-allure'
        APP_REPOSITORY = 'https://github.com/EBAC-QE/hub-de-leitura-integrado2.git'
        TEST_REPOSITORY = 'https://github.com/richielmartillo/hub-de-leitura-ui-cypress-allure.git'
    }

    stages {
        stage('Clonar e iniciar a aplicacao') {
            steps {
                bat '''
                    if exist "%APP_DIR%" rmdir /s /q "%APP_DIR%"
                    git clone --depth 1 "%APP_REPOSITORY%" "%APP_DIR%"
                    cd /d "%APP_DIR%"
                    call npm ci
                    start "Hub de Leitura" /B cmd /c "npm start > app.log 2>&1"
                '''
            }
        }

        stage('Clonar e instalar o projeto de teste') {
            steps {
                bat '''
                    if exist "%TEST_DIR%" rmdir /s /q "%TEST_DIR%"
                    git clone --depth 1 "%TEST_REPOSITORY%" "%TEST_DIR%"
                    cd /d "%TEST_DIR%"
                    call npm ci
                '''
            }
        }

        stage('Esperar a aplicacao subir') {
            steps {
                dir(env.TEST_DIR) {
                    bat 'call npx wait-on http://localhost:3000'
                }
            }
        }

        stage('Executar testes no Cypress Cloud') {
            steps {
                dir(env.TEST_DIR) {
                    withCredentials([string(credentialsId: 'cypress-record-key', variable: 'CYPRESS_RECORD_KEY')]) {
                        bat '''
                            call npx cypress run ^
                                --record ^
                                --key %CYPRESS_RECORD_KEY% ^
                                --browser chrome ^
                                --ci-build-id jenkins-%BUILD_NUMBER% ^
                                --group "UI-Windows"
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            dir(env.TEST_DIR) {
                archiveArtifacts artifacts: 'cypress/screenshots/**/*.*,cypress/videos/**/*.*,allure-results/**/*', allowEmptyArchive: true
                allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
            }
            bat 'taskkill /F /IM node.exe >nul 2>&1 || exit /b 0'
        }
    }
}
