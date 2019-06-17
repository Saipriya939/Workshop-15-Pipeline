pipeline {
    agent { label 'ca-brightside-ce-agent' }
    environment {
        // Scripts
        BUILD = "./jenkins/build.sh"
        DEPLOY = "./jenkins/deploy.sh"
        TEST = "./jenkins/test.sh"

        //Endevor environment variables
        ENDEVOR_CONNECTION="--port 6002 --protocol http --reject-unauthorized false"
        ENDEVOR_LOCATION="--instance ENDEVOR --env DEV --sys MARBLES --sub MARBLES --stage-number 1 --ccid JENK01 --comment JENK01"
        ENDEVOR="$ENDEVOR_CONNECTION $ENDEVOR_LOCATION"
        //mainframe host ip address
        ZOWE_OPT_HOST=credentials('eoshost')
    }
    stages {
        stage('local setup') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'bright --version'
                sh 'bright plugins list'
                sh 'npm install gulp-cli -g'
                sh 'npm install'
            }
        }
        stage('build') {
            steps {
                //sh "chmod +x $BUILD && $BUILD"
                //ZOWE_OPT_USERNAME & ZOWE_OPT_PASSWORD are used to interact with Endevor 
                 withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                    
                   sh 'gulp build'
                 }
            }
        }
        stage('deploy') {
            steps {
                sh "chmod +x $DEPLOY && $DEPLOY"
                //ZOWE_OPT_USER & ZOWE_OPT_PASSWORD are used to interact with z/OSMF and CICS
                // withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                //     //ZOWE_OPT_PASS is used by FMP plugin
                //     withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASS')]) {
                //        
                //     }
                // }
            }
        }
        stage('test') {
            steps {
                sh "chmod +x $TEST && $TEST"
                //ZOWE_OPT_USER & ZOWE_OPT_PASS are used to interact with z/OSMF
                // withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                // }
            }
        }
    }

    // post {
    //     always {
    //         // Archive build artifacts
    //         archiveArtifacts artifacts: 'endevor-report-*', fingerprint:true
    //         // Archive test report
    //         publishHTML([allowMissing: false,
    //             alwaysLinkToLastBuild: true,
    //             keepAll: true,
    //             reportDir: 'mochawesome-report',
    //             reportFiles: 'mochawesome.html',
    //             reportName: 'Test Results',
    //             reportTitles: 'Test Report'
    //             ])
    //     }
    // }
}