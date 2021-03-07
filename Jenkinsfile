pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Compile') {
      steps {
        echo 'Compiling...'
        sh "${tool name: 'sbt', type:'org.jvnet.hudson.plugins.SbtPluginBuilder$SbtInstallation'}/bin/sbt compile"
        sh 'echo "${COMPILER}"'
      }
    }

    stage('Packaging') {
      parallel {
        stage('Packaging') {
          steps {
            echo 'Packaging...'
            sh "${tool name: 'sbt', type:'org.jvnet.hudson.plugins.SbtPluginBuilder$SbtInstallation'}/bin/sbt clean stage"
          }
        }

        stage('stash') {
          steps {
            stash(name: 'packaged', includes: 'target/**/*.jar')
          }
        }

      }
    }

    stage('error') {
      parallel {
        stage('error') {
          steps {
            archiveArtifacts(artifacts: 'target/**/*.jar', fingerprint: true)
          }
        }

        stage('unstash') {
          steps {
            unstash 'packaged'
          }
        }

      }
    }

  }
  environment {
    COMPILER = 'sbt'
  }
}