#!groovy

docker.image("cloudbees/java-build-tools:0.0.7.1").inside {

    git credentialsId: 'github-ssh-credentials', url: 'git@github.com:cyrille-leclerc/my-spring-boot-gradle-app.git'

    wrap([$class: 'ConfigFileBuildWrapper', managedFiles: [[fileId: 'gradle-properties-for-my-spring-boot-gradle-app', targetLocation: 'gradle.properties', variable: '']]]) {

        stage 'Compile, test and deploy'
        // GRADLE_USER_HOME env var and '--gradle-user-home' are equivalents
        sh """
        ./gradlew --gradle-user-home=${pwd()}/.gradle clean build test publish
        """
        step([$class: 'ArtifactArchiver', artifacts: '**/build/libs/*.jar'])
        step([$class: 'JUnitResultArchiver', testResults: 'build/test-results/TEST-*.xml'])
    }
}