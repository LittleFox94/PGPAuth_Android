// Every jenkins file should start with either a Declarative or Scripted Pipeline entry point.
node {
    // Global variable declaration
    def project = 'pgpauth'
    def appName = 'PGPAuth'

    // Stage, is to tell the Jenkins that this is the new process/step that needs to be executed
    stage('Checkout') {
        // Pull the code from the repo
        checkout scm
    }

    stage('Build Image') {
        // Build our docker Image
        sh("docker build -t ${project} .")
    }

//   stage('Run application test') {
//       // If you need environmental variables in your image. Why not load it attach it to the image, and delete it afterward
//       sh("env >> .env")
//       sh("docker run --env-file .env --rm ${project} ./gradlew test")
//       sh("rm -rf .env")
//   }

    stage('Deploy application') {
        // This is the cool part where you deploy. Here, you can specify builds you want to deploy
        switch (env.BRANCH_NAME) {
            case "master":
                sh("env >> .env")
                sh("docker run --env-file .env --rm ${project} ./gradlew clean build assembleRelease")
                sh("rm -rf .env")
                break
            default:
                sh("env >> .env")
                sh("docker run --env-file .env --rm ${project} ./gradlew clean build assembleDebug")
                sh("rm -rf .env")
                break
        }
    }
}
