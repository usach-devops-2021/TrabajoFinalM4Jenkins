pipeline {
    agent any

    
    stages {
        stage("Paso 01: Download and checkout API"){
            steps {
                checkout(
                    [$class: 'GitSCM',
                    //Acá reemplazar por el nonbre de branch
                    branches: [[name: "main" ]],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'TrabajoFinalM4SWD']],
                    //Acá reemplazar por su propio repositorio
                    userRemoteConfigs: [[url: 'https://github.com/usach-devops-2021/TrabajoFinalM4SWD.git']]])
            }
        }
        stage("Paso 02: Download and checkout Front"){
            steps {
                checkout(
                    [$class: 'GitSCM',
                    //Acá reemplazar por el nonbre de branch
                    branches: [[name: "main" ]],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'TrabajoFinalM4Front']],
                    //Acá reemplazar por su propio repositorio
                    userRemoteConfigs: [[url: 'https://github.com/usach-devops-2021/TrabajoFinalM4Front.git']]])
            }
        }
        stage("Paso 1: Compilar"){
            steps {
                dir("TrabajoFinalM4SWD") {
                    script {
                    sh "echo 'Compile Code!'"
                    // Run Maven on a Unix agent.
                    sh "mvn clean compile -e"
                    }
                }
            }
        }
        stage("Paso 2: Testear"){
            steps {
                dir("TrabajoFinalM4SWD") {
                    script {
                    sh "echo 'Test Code!'"
                    // Run Maven on a Unix agent.
                    sh "mvn clean test -e"
                    }
                }
            }
        }
        stage("Paso 3: Build .Jar"){
            steps {
                dir("TrabajoFinalM4SWD") {
                    script {
                    sh "echo 'Build .Jar!'"
                    // Run Maven on a Unix agent.
                    sh "mvn clean package -e"
                    }
                }
            }
        }
        stage("Paso 4: Levantar Springboot APP"){
            steps {
                dir("TrabajoFinalM4SWD") {
                    sh 'mvn spring-boot:run &'
                }
            }
        }
        stage("Paso 5: Dormir(Esperar 10sg) "){
            steps {
                sh 'sleep 10'
            }
        }
        stage("Paso 6: Curl con Sleep de prueba "){
            steps {
               sh "curl -X GET 'http://localhost:8081/rest/msdxc/dxc?sueldo=500000&&ahorro=25000000'"
            }
        }
        stage("Paso 7: Test Jmeter"){
            steps {
                dir("TrabajoFinalM4SWD") {
                    sh 'mvn jmeter:jmeter -Pjmeter'
                }
            }
        }
        stage('Paso 8: Test API responses') {
            steps {
                dir("TrabajoFinalM4SWD") {
                    sh "newman run LabMod4.postman_collection.json"
                }
            }
        }
        stage("Paso 9: Install npm modules"){
            steps {
                dir("TrabajoFinalM4Front") {
                    script {
                    sh "echo 'npm install!'"
                    // Run Maven on a Unix agent.
                    sh "npm install"
                    }
                }
            }
        }
        stage("Paso 10: Start Node Server"){
            steps {
                dir("TrabajoFinalM4Front") {
                    script {
                    sh "echo 'npm app!'"
                    // Run Maven on a Unix agent.
                    sh "node app.js &"
                    }
                }
            }
        }
        stage("Paso 11: Dormir(Esperar 10sg) "){
            steps {
                sh 'sleep 10'
            }
        }
        stage("Paso 12: Curl con Sleep de prueba "){
            steps {
               sh "curl -X GET 'http://localhost:3000/'"
            }
        }
        
    }
}