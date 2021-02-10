node('linux') {
    withCredentials([usernamePassword(credentialsId: 'NPA_BUILDER', passwordVariable: 'npa_password', usernameVariable: 'npa_user')]) {
        stage('Checkout') {
            checkout scm
        }
        stage('Clean') {
            sh "sh ./gradlew -Dgradle.user=${npa_user} -Dgradle.password=${npa_password} clean"
        }
        stage('Build') {
            when {
                not {
                    branch 'main'
                }
            }
            sh "sh ./gradlew -Dgradle.user=${npa_user} -Dgradle.password=${npa_password} build"
        }
        stage('Release') {
            when {
                branch 'main'
            }
            withCredentials([usernamePassword(credentialsId: 'NPA_BUILDER', passwordVariable: 'GRGIT_PASS', usernameVariable: 'GRGIT_USER')]) {
                sh "sh ./gradlew -Dgradle.user=${npa_user} -Dgradle.password=${npa_password} release"
            }
        }
    }
}
