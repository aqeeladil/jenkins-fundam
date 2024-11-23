# Jenkins Shared Libraries

Jenkins Shared Libraries allow you to share reusable code across multiple Jenkins pipelines. They are particularly useful when you have repetitive tasks or standardized workflows across multiple projects. Instead of duplicating the same logic across different pipelines, you can define it once in a shared library and reference it wherever needed. By centralizing common functions, you make your pipelines cleaner, reduce duplication, and simplify maintenance.

___________________________________________________________________________________________

## Advantages

- Standarization of Pipelines
- Reduce duplication of code
- Easy onboarding of new applications, projects or teams
- One place to fix issues with the shared or common code
- Code Maintainence 
- Reduce the risk of errors

![Screenshot 2023-05-02 at 9 47 24 PM](https://user-images.githubusercontent.com/43399466/235724851-90a5cad6-ac0d-428b-9944-93fffea55180.png)

______________________________________________________________________________________________

## Structure of a Shared Library

A shared library typically follows a structured format in a Git repository.
```scss
(root of the repository)
├── vars/
│   ├── exampleFunction.groovy      
│   └── anotherFunction.groovy
├── resources/
│   └── someScript.sh
├── src/
│   └── org/
│       └── mycompany/
│           └── mylibrary/
│               └── Helper.groovy
├── README.md
└── build.gradle (optional)
```
- ```vars/```: Contains reusable pipeline scripts or functions (global variables) accessible in Jenkins pipelines.
- ```resources/```: Stores non-Groovy resources, such as shell scripts, configuration files, or templates.
- ```src/```: Contains structured Groovy code for complex logic or libraries.
- ```README.md```: Documents library functionality.
- ```build.gradle```: Optional, for managing Groovy builds.

## Setting Up a Shared Library in Jenkins

1. Set up a Git repository for the shared library with the folder structure mentioned above.
2. Go to Manage Jenkins > Configure System > Global Pipeline Libraries.
3. Add a new library:
- Library Name: A unique identifier (e.g., ```my-shared-library```).
- Default Version: Specify ```master``` or a version/tag.
- Retrieval Method: Choose the repository (e.g., Git) and provide the URL and credentials if needed.

### Example-1: Creating a Simple Function in a Shared Library

Create a Function in ```vars/exampleFunction.groovy```
```groovy
def call() {
    echo 'Hello from the shared library!'
}
```
Use the Function in a Pipeline
```groovy
@Library('my-shared-library') _
pipeline {
    agent any
    stages {
        stage('Greeting') {
            steps {
                script {
                    exampleFunction()
                }
            }
        }
    }
}
```

### Example-2: Creating Custom Class using Groovy classes in the ```src/``` folder.

```groovy
package org.mycompany.mylibrary
class Helper {
    static String sayHello(String name) {
        return "Hello, ${name}!"
    }
}
```
Use in your pipeline:
```groovy
@Library('my-shared-library') _
import org.mycompany.mylibrary.Helper

pipeline {
    agent any
    stages {
        stage('Greeting') {
            steps {
                script {
                    echo Helper.sayHello('Jenkins')
                }
            }
        }
    }
}
```
