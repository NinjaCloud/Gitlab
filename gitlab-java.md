## 🔧 Lab Objective:

* Create a Java `HelloWorld` program.
* Write a `GitLab CI/CD` pipeline to compile and run it.
* Validate that GitLab Runner can build and execute Java code.

---

## 🗂 Project Structure

```
hello-world-java-gitlab/
├── .gitlab-ci.yml
└── HelloWorld.java
```

---

## 📄 Step 1: Create `HelloWorld.java`

```java
// HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World from Java!");
    }
}
```

---

## ⚙️ Step 2: Create `.gitlab-ci.yml`

This YAML file defines a simple pipeline that uses a Docker image with Java (e.g., OpenJDK) to compile and run the code.

```yaml
# .gitlab-ci.yml
stages:
  - build
  - run

variables:
  JAVA_FILE: "HelloWorld.java"
  CLASS_FILE: "HelloWorld"

build:
  stage: build
  image: openjdk:17
  script:
    - javac $JAVA_FILE
  artifacts:
    paths:
      - $CLASS_FILE.class

run:
  stage: run
  image: openjdk:17
  script:
    - java $CLASS_FILE
```



## ✅ Output

You’ll see the output in the job logs:

```
Hello, World from Java!
```

---

