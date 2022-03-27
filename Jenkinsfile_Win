pipeline {
    agent any
    stages {
        stage('Run tests') {
            steps {
                bat """
                    docker run -v="G:\\code":/project -v="G:\\code":/reports -e COMMAND_LINE="-f/reports '/project/project.xml'" -i smartbear/soapuios-testrunner:latest > "G:\\code\\output.txt"    
                """
            }
        }
    }
}