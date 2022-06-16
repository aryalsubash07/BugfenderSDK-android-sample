def getPropOrDefault( Closure c, def defaultVal ) {
    try {
        return c()
    }
    catch( groovy.lang.MissingPropertyException e ) {
        return defaultVal
    }
}
pipeline {
    agent any
    environment {
        APP_NAME = 'TestApp'
        BRANCH = 'develop'
        VERSION_CODE = 1
        VERSION_NAME = '1.0.0'
        BUILD_TYPE = 'Release'
        BUNDLE_TYPE= 'assemble'
        EXPO_TOKEN = credentials('Apad-tokens')
    }
    stages {
        stage('Prepare') {
            steps {
                echo "Preparing Build Jobs"
            }
        }
        stage('setup') {
            steps {
                echo "Setup Build Jobs"
            }
        }
        stage("Inject Environment Variable") {
            steps {
                echo "Inject Environment Variable"
                 sh "echo -e 'versionCode=${VERSION_CODE}\nversionName=${VERSION_NAME}' > android/app_version.properties"

            }
        }
        stage("Clean"){
            steps {
                echo "Clean Project"
                 script {
                       sh """
                         #!/bin/bash -e
                         echo "--------- Building Project  -------------------"
                         who
                         ./gradlew clean
                     """
                 }
            }
        }
        stage('Build') {
          steps {
            echo "Build Project"
             // Finish building and packaging the APK/AAB
             sh """
                 #!/bin/bash -e
                 echo "--------- Building APK/AAB  -------------------"
                 pwd
                 ./gradlew ${BUNDLE_TYPE}${BUILD_TYPE}
             """
             // Archive the APKs so that they can be downloaded from Jenkins
             archiveArtifacts artifacts:'**/*.apk,**/*.aab'
          }
        }
    }
}
