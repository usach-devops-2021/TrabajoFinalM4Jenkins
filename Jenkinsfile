pipeline {
    agent any

    
    stages {
        stage("Paso 1: Download and checkout API"){
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
        stage("Paso 2: Download and checkout Front"){
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
        stage("Paso 3: Install npm modules"){
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
        stage("Paso 4: Start Node Server"){
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
        stage("Paso 5: Dormir (Esperar 15seg) (Front)"){
            steps {
                sh 'sleep 15'
            }
        }
        stage("Paso 6: Curl con Sleep de prueba (Front)"){
            steps {
               sh "curl -X GET 'http://localhost:3000/'"
            }
        }
        stage("Paso 7: Compilar"){
            steps {
                //dir("TrabajoFinalM4SWD") {
                    script {
                        sh "echo 'Compile Code!'"
                        // Run Maven on a Unix agent.
                        sh "mvn clean compile -e"
                    }
                //}
            }
        }
        stage('Paso 8: Levantar Springboot APP (Back) para realizar pruebas siguientes') {
            steps {
                sh 'mvn spring-boot:run &'
            }
        }
        stage('Paso 9: Dormir(Esperar 60seg) (Back)') {
            steps {
                sh 'sleep 60'
            }
        }
        stage('Paso 10: Curl con Sleep de prueba (Back)') {
            steps {
                sh 'curl -X GET "http://localhost:8081/rest/msdxc/ping"'
            }
        }
        stage("Paso 11: Testear (Back)"){
            steps {
                dir("TrabajoFinalM4SWD") {
                    script {
                        sh "echo 'Test Code!'"
                        sh "chmod +x src/driver/linx/chromedriver"
                        // Run Maven on a Unix agent.
                        sh "mvn clean test -e"
                    }
                }
            }
        }
        stage("Paso 12: Build .Jar (Back)"){
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
        /*
        stage("Paso 10: Levantar Springboot APP"){
            steps {
                dir("TrabajoFinalM4SWD") {
                    sh 'mvn spring-boot:run &'
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
               sh "curl -X GET 'http://localhost:8081/rest/msdxc/dxc?sueldo=500000&&ahorro=25000000'"
            }
        }
        */
        stage("Paso 13: Test Jmeter (Back)"){
            steps {
                dir("TrabajoFinalM4SWD") {
                    sh 'mvn jmeter:jmeter -Pjmeter'
                }
            }
        }
        stage('Paso 14: Test API responses (Back)') {
            steps {
                dir("TrabajoFinalM4SWD") {
                    sh "newman run LabMod4.postman_collection.json"
                }
            }
        }  
        
    }
}
