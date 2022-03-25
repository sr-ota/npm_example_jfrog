def rtServer, buildInfo

pipeline {
    agent {
        any {
        }
    }
    //parameters {
    //    string (name: 'ART_URL', defaultValue: 'https://evaluate.jfrog.io/artifactory', description: 'URL for JFrog Artifactory')
    //}
    environment {
        CI = true
        ARTIFACTORY_ACCESS_TOKEN = credentials('JFROG_ACCESS_TOKEN')
        ART_URL = "https://evaluate.jfrog.io/artifactory"
        BUILD_NAME = "npm-cli-build"
        BUILD_ID = 3
    }
    stages {
        stage('Build') { 
            steps {
                sh 'jf rt build-add-git $BUILD_NAME $BUILD_ID'
                sh 'jf npm install --build-name $BUILD_NAME --build-number $BUILD_ID' 
                sh 'jf rt build-add-dependencies $BUILD_NAME $BUILD_ID "node_modules/**/*"'
            }
        }
        stage('Xray Scan'){
            steps {
                sh 'ls'
                // sh 'jf rt upload target/marathon.war Marathon-App-Jenkins --url ${ART_URL} --access-token ${ARTIFACTORY_ACCESS_TOKEN} --build-name $BUILD_NAME --build-number $BUILD_ID'
           }
        }
        stage('JFrog Build Publish'){
            steps {
                sh 'jf rt build-publish $BUILD_NAME $BUILD_ID --url $ART_URL --access-token $ARTIFACTORY_ACCESS_TOKEN'
           }
        }
    }
}
