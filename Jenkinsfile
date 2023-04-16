pipeline {
    agent any
    stages {
        stage("Starting Server for Image Build") {
            steps {
                sh "gcloud compute instances start ansible --project trackier-mmp --zone europe-west1-c"
                sh "sleep 5"
            }
        }
        stage("Starting Build") {
            steps {
                script {
                    TAG = VersionNumber(versionNumberString: '${BUILD_DATE_FORMATTED, "yyyyMMdd"}-${BUILDS_TODAY}')
                }
                retry(3) {
                    sh """gcloud compute ssh --tunnel-through-iap --zone europe-west1-c --project trackier-mmp ansible -- 'sudo -H -u root bash -c "/root/event-srv-mmp/test.sh ${TAG}"'"""
                    sleep time: 20, unit: 'SECONDS'
                }
            }
        }
    }
}
