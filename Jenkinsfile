node {
    try {
        def mvnHome = tool 'M3'
        env.JAVA_HOME="${tool 'jdk-oracle-8'}"
        env.PATH="${env.JAVA_HOME}/bin:${mvnHome}/bin:${env.PATH}"

        stage 'Clone'
        checkout scm

        def gitTag = sh(returnStdout: true, script: "git rev-parse --short HEAD").trim()


        stage 'Deploy'
        sh "mvn deploy:deploy-file -DgroupId=org.rascalmpl -DartifactId=update-site-script -Dversion=1.0.0-SNAPSHOT -Dclassifier=${gitTag} -DgeneratePom=false -Dpackaging=sh -DrepositoryId=usethesource-snapshots -Durl=http://nexus.usethesource.io/content/repositories/snapshots/ -Dfile=refresh-nexus-data"

    } catch(e) {
        slackSend (color: '#d9534f', message: "FAILED: <${env.BUILD_URL}|${env.JOB_NAME} [${env.BUILD_NUMBER}]>")
        throw e
    }

}
