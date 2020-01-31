pipeline {
    agent {
        label "master"
    }
    environment {
        FILE_NAME = "netboot.xyz.kpxe"
        SOURCE_URL = "https://boot.netboot.xyz/ipxe/${FILE_NAME}"
        MIKROTIK_USER = credentials("MIKROTIK_USER")
        MIKROTIK_PASSWORD = credentials('MIKROTIK_PASSWORD')
    }
    stages {
        stage("Invoking curl") {
            steps {
                sh ([["curl", "--output", FILE_NAME, SOURCE_URL],
                     ["sha256sum", FILE_NAME],
                     ["curl", "--upload-file", FILE_NAME,
                      "--user", "${MIKROTIK_USER}:${MIKROTIK_PASSWORD}",
                      "ftp://192.168.105.1/${FILE_NAME}"]]
                    .collect{it.join(" ")}
                    .join("; "))
            }
        }
    }
    post {
        always {
            sendNotifications currentBuild.result
        }
    }
}
