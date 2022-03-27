# QA-Automation-Challenge

## Tabla de contenido
- [Summary of what has been done](#summary-of-what-has-been-done).
- [How to run the docker from Jenkins step by step](#how-to-run-the-docker-from-jenkins-step-by-step).
- [Results of my execution](#results-of-my-execution).
- [What could be improved from the work performed](#what-could-be-improved-from-the-work-performed).
- [What the task can be used for?](#what-the-task-can-be-used-for?).

## Summary of what has been done

First of all, no new Docker image has been created because the software used for the creation of the TestSuite has been SoapUI and, in the [official SoapUI documentation](https://www.soapui.org/docs/test-automation/running-in-docker/), there is already an image used for this task.

For this task a project has been created with a TestSuite of four back end cases. These four cases test a small part of a very basic API of a calculator. The project can be found in this repository under the name "project.xml".

The four cases have been prepared in such a way that two of them will pass the assertions and the other two will not, so when we launch the TestSuite it will appear with failed tests.

Below is a quick explanation of each of the four cases:
- TC01.- Add two numbers and multiply that result - Passed Case -> It does the sum of two numbers and the result multiplies them. At the beginning of the test there is a step ("Properties") where we give value to the data we want to operate with and the expected results. This case, like TC02, has the assertions within each of the Soap steps.
- TC02 -> The flow is the same as TC01. For this case it has been forced to fail and for this reason it has been given in the parameters, step "Properties", a different number than the correct one. Like TC01, it has the assertions inside each of the Soap steps.
- TC03 -> With the help of a Groovy script we send requests to the Add service incriminating the input parameters of the Request until the service fails for exceeding the maximum number of characters. In this way we check the maximum number of characters that the service supports for both operators. For this case and for TC04 the assertions are in the Groovy script (this way we see a second way to create assertions).
- TC04 -> The flow is the same as for TC03. For this case it has been forced to fail and for this purpose it has been given in the parameters, step "Properties", a different maximum number of characters than the one accepted by the input parameter. For this case and for TC03 the assertions are in the Groovy script (this way we see a second way to create assertions).

The next step, having already the docker image provided by the official SoapUI documentation, has been the creation of a pipeline in Jenkins to run the Docker image and in this way launch the TestSuite from Jenkins.

For this task we have created two pipelines that do exactly the same, the difference is the command that executes the instruction to run the Docker image because it changes depending on the Operating System (OS) where we have Jenkins ("bat" for Windows OS and "sh" for Linux OS).

The pipeline for Windows OS is in the "Jenkinsfile_Win" file attached in this repository.

The Linux OS pipeline is in the attached "Jenkinsfile_Lin" file in this repository.

The Pipeline in Jenkins completes the explanation of the scope of this task.

## How to run the docker from Jenkins step by step
### Requirements
- We need to have Jenkins configured with Docke plugin installed.
### Step by step
- Create a new folder with the name "qachallenge" in the root of disk C.
- Download the "project.xml" file found in this repository and save it in the path "C:\qachallenge".
- Access Jenkins and create a new pipeline with the following code:
####     Windows SO:
```
pipeline {
    agent any
    stages {
        stage('Run tests') {
            steps {
                bat """
                    docker run -v="C:\\qachallenge":/project -v="C:\\qachallenge":/reports -e COMMAND_LINE="-f/reports '/project/project.xml'" -i smartbear/soapuios-testrunner:latest > "C:\\qachallenge\\output.txt"    
                """
            }
        }
    }
}
```
          
####     Linux SO:
```
pipeline {
    agent any
    stages {
        stage('Run tests') {
            steps {
                sh """
                    docker run -v="C:\\qachallenge":/project -v="C:\\qachallenge":/reports -e COMMAND_LINE="-f/reports '/project/project.xml'" -i smartbear/soapuios-testrunner:latest > "C:\\qachallenge\\output.txt"      
                """
            }
        }
    }
}
```
- The pipeline can now be executed. Click on the "Build Now" option.
          
## Results of my execution

- My execution of the task will be shown with the help of images. I will first show how to launch it manually from SoapUI and then the execution from Jenkins to see how the logs are displayed in both cases.
- The following image shows in SoapUI the TestSuite with the four cases and their respective steps.

![imagen](https://user-images.githubusercontent.com/29427746/160293815-431168ac-58e8-4954-adeb-bbebcccacde3.png)

- As input data and expected data are indicated. Note that the "FinalResult" data is automatically filled with the final result obtained from the execution.

![imagen](https://user-images.githubusercontent.com/29427746/160293926-cf1c0072-77dd-4b7e-ab57-1e5fa14c0e95.png)

- The TestSuite is run and the four cases are launched sequentially. The cases could be launched in parallel since no case depends on the other but SoapUI does not keep logs in this case.The following image shows the result of the manual execution.

![imagen](https://user-images.githubusercontent.com/29427746/160294297-3c9ea093-2e7e-4cb8-94cb-a383b9d8ee1a.png)

- Execution logs

![imagen](https://user-images.githubusercontent.com/29427746/160295188-cb14a31e-6715-46df-ab9b-bd3a8a96ac71.png)


- Now the same execution is performed but from Jenkins with the help of the Docker image provided by SoapUI.
- Create a new folder with the name "qachallenge" in the root of disk C.
- Download the "project.xml" file found in this repository and save it in the path "C:\qachallenge".
- 
![imagen](https://user-images.githubusercontent.com/29427746/160293683-69c8fb0a-20d2-4c54-8c2e-d0f9eb7f165c.png)

- Finally go to Jenkins, create a new Pipeline and add the code according to the corresponding OS. In my case it is Windows OS.

![imagen](https://user-images.githubusercontent.com/29427746/160294855-d5f5f218-79c3-4df0-8079-9cb0da5e7172.png)

- Apply, save and run the Pipeline.
- The execution fails because we have tests that have not passed all assertions.

![imagen](https://user-images.githubusercontent.com/29427746/160295045-62246992-623d-42f8-92aa-71b5b84ff6c5.png)

- You can check the logs in Jenkins as shown in the following image.

![imagen](https://user-images.githubusercontent.com/29427746/160295204-b0bd1a4c-ff90-4672-a4df-6b071ec22db9.png)

- And if you go back to the folder where you have the project you will see new files. One, with all the log of the execution. And other two for each one of the failed cases in particular. These files are attached in the repository (".

![imagen](https://user-images.githubusercontent.com/29427746/160295343-47d7330d-2258-40d8-8b27-335725d9f9c0.png)









          
        
