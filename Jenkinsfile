pipeline {
  agent any
  tools {
    jdk "jdk"
  }
  stages {
    stage('Build') {
        steps {
            echo "Build repo"
            bat "./mvnw clean package -Dmaven.test.skip=true"
        }
    }
    stage('Test') {
        steps {
            echo "Testing"
            bat "./mvnw test"
        }
    }
    stage('Build docker image') {
        steps {
            echo "Build docker image"
            bat "docker build --build-arg JAR_FILE=target/*.jar -t petclinic/app ."
        }
    }
    stage('Run application') {
        steps {
            echo "Run application"
            bat "start /B docker run -p 8080:8080 petclinic/app"
        }
    }
  }
  post {
    success {
        jacoco(
            execPattern: '**/target/jacoco/*.exec',
            classPattern: '**/target/classes/org/springframework',
            sourcePattern: '**/src/main'
        )
    }
  }
}
