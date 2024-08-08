pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Use 'bat' instead of 'sh' for Windows
                bat 'pipenv --python C:/Users/bkart/AppData/Local/Programs/Python/Python310/python.exe sync'
            }
        }
        stage('Test') {
            steps {
                // Use 'bat' instead of 'sh' for Windows
                bat 'pipenv run pytest'
            }
        }
        stage('Package') {
            when {
                anyOf { branch "master"; branch 'release' }
            }
            steps {
                // Use 'bat' instead of 'sh' for Windows
                bat 'powershell Compress-Archive -Path lib -DestinationPath sbdl.zip'
            }
        }
        stage('Release') {
            when {
                // Use a condition that is never true to skip this stage
                expression { return false }
            }
            steps {
                // Use 'bat' instead of 'sh' for Windows
                bat """
                pscp -i C:\\path\\to\\edge-node_key.ppk -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-qa
                """
            }
        }
        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                // Use 'bat' instead of 'sh' for Windows
                bat """
                pscp -i C:\\path\\to\\edge-node_key.ppk -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-prod
                """
            }
        }
    }
}