pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MAVEN_HOME"
    }

    stages {
        stage('Clone') {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    // Especifica la rama master explícitamente para mayor claridad.
                    git branch: 'main', url: 'https://github.com/zetaxdxd/demo-crud-repaso.git'
                }
            }
        }
        stage('Build') {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    // Corregido: El pom.xml está en la raíz del workspace después del clone
                    sh "mvn -DskipTests clean package"
                }
            }
        }
        stage('Test') {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    // Se cambia <test> por <install> para que se genere el reporte de jacoco
                    // Corregido: El pom.xml está en la raíz del workspace
                    sh "mvn clean install"
                }
            }
        }
        stage('Sonar') {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    withSonarQubeEnv('sonarqube'){ // Asegúrate que 'sonarqube' es el nombre de tu server SonarQube en la config de Jenkins
                        sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar -Pcoverage"
                    }
                }
            }
        }
        stage('Quality gate') {
            steps {
                sleep(10) //seconds 

                timeout(time: 10, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Comando de deploy sería algo como: mvn spring-boot:run"
                // Aquí iría tu comando de deploy real si lo tuvieras.
                // sh "mvn spring-boot:run -f pom.xml" // Si fuera a ejecutarlo
            }
        }
    }
}

