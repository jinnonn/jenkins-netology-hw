# Домашнее задание к занятию «Что такое DevOps. СI/СD»

### Задание 1

**Что нужно сделать:**

1. Установите себе jenkins по инструкции из лекции или любым другим способом из официальной документации. Использовать Docker в этом задании нежелательно.
2. Установите на машину с jenkins [golang](https://golang.org/doc/install).
3. Используя свой аккаунт на GitHub, сделайте себе форк [репозитория](https://github.com/netology-code/sdvps-materials.git). В этом же репозитории находится [дополнительный материал для выполнения ДЗ](https://github.com/netology-code/sdvps-materials/blob/main/CICD/8.2-hw.md).
3. Создайте в jenkins Freestyle Project, подключите получившийся репозиторий к нему и произведите запуск тестов и сборку проекта ```go test .``` и  ```docker build .```.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

---

### Решение 1

![image](https://github.com/jinnonn/jenkins-netology-hw/assets/146999555/38326f35-ac11-481c-adde-a74339de9c66)
![image](https://github.com/jinnonn/jenkins-netology-hw/assets/146999555/0871c085-c708-4944-8cb1-17f99e4cd8c9)
![image](https://github.com/jinnonn/jenkins-netology-hw/assets/146999555/a74b02f9-de56-4d48-81be-e9255407852d)
![image](https://github.com/jinnonn/jenkins-netology-hw/assets/146999555/0ac65d2b-e15f-4d9f-923c-e18a32487ef4)

---

### Задание 2

**Что нужно сделать:**

1. Создайте новый проект pipeline.
2. Перепишите сборку из задания 1 на declarative в виде кода.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

---

### Решение 2

![image](https://github.com/jinnonn/jenkins-netology-hw/assets/146999555/adeaae59-ef9b-4e58-9886-6be23af1bb28)
![image](https://github.com/jinnonn/jenkins-netology-hw/assets/146999555/f46cef7e-2e9b-45e2-b8b0-a74f060e4a97)

---

### Задание 3

**Что нужно сделать:**

1. Установите на машину Nexus.
2. Создайте raw-hosted репозиторий.
3. Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
4. Загрузите файл в репозиторий с помощью jenkins.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

---

### Решение 3
```
Started by user BobkovEO
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/my_pipeline_2
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Git)
[Pipeline] git
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/my_pipeline_2/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/jinnonn/sdvps-materials-hw.git # timeout=10
Fetching upstream changes from https://github.com/jinnonn/sdvps-materials-hw.git
 > git --version # timeout=10
 > git --version # 'git version 2.34.1'
 > git fetch --tags --force --progress -- https://github.com/jinnonn/sdvps-materials-hw.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 801b1f128eaff6503492ad44b28d839948a3b929 (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 801b1f128eaff6503492ad44b28d839948a3b929 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D main # timeout=10
 > git checkout -b main 801b1f128eaff6503492ad44b28d839948a3b929 # timeout=10
Commit message: "Update README.md"
 > git rev-list --no-walk 801b1f128eaff6503492ad44b28d839948a3b929 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] sh
+ CGO_ENABLED=0 GOOS=linux /usr/local/go/bin/go build -a -installsuffix nocgo -o hello-world .
[Pipeline] sh
+ curl -u admin:1234 http://51.250.110.180:8081/repository/netology-ci_cd-hw/ --upload-file hello-world
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 1874k    0     0  100 1874k      0   9.8M --:--:-- --:--:-- --:--:--  9.7M
100 1874k    0     0  100 1874k      0   9.8M --:--:-- --:--:-- --:--:--  9.7M
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```
![image](https://github.com/jinnonn/jenkins-netology-hw/assets/146999555/9d3b012e-1aa7-402d-9e99-598307ae5edb)
```
pipeline {
    agent any
    stages {
        stage('Git') {
            steps {
                git url: 'https://github.com/jinnonn/sdvps-materials-hw.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'CGO_ENABLED=0 GOOS=linux /usr/local/go/bin/go build -a -installsuffix nocgo -o hello-world .'
                sh 'curl -u "admin:1234" http://51.250.110.180:8081/repository/netology-ci_cd-hw/ --upload-file hello-world'
            }
        }
    }
}
```
![image](https://github.com/jinnonn/jenkins-netology-hw/assets/146999555/bb5b4b76-da42-4a1a-ac84-12815125ddad)


