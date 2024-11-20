### Инструкция по настройке Jenkins и запуску пайплайна

#### 1. Установка Jenkins

1. **Установите Java**: Jenkins требует Java для работы. Убедитесь, что Java установлена на вашем сервере:

   ```sh
   sudo apt update
   sudo apt install openjdk-11-jdk
   ```

2. **Установите Jenkins**:

   ```sh
   wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
   sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   sudo apt update
   sudo apt install jenkins
   ```

3. **Запустите Jenkins**:

   ```sh
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   ```

4. **Откройте Jenkins в браузере**:

   Откройте браузер и перейдите по адресу `http://<your-server-ip>:8080`. Следуйте инструкциям на экране, чтобы завершить установку.

#### 2. Настройка Jenkins

1. **Создайте новый Item**:

   - В Jenkins выберите "New Item".
   - Введите имя для вашего пайплайна и выберите "Pipeline".
   - Нажмите "OK".

2. **Настройка Build Triggers**:

   - В разделе "Build Triggers" включите "Poll SCM".
   - В поле "Schedule" введите `*/5 * * * *`, чтобы Jenkins опрашивал репозиторий каждые 5 минут.

3. **Настройка Pipeline**:

   - В разделе "Pipeline" выберите "Pipeline script from SCM".
   - В качестве SCM выберите "Git".
   - В поле "Repository URL" укажите URL вашего Git-репозитория.
   - Создайте новые Credentials для доступа к репозиторию:
     - Выберите "Add" -> "Jenkins".
     - В качестве "Kind" выберите "Username with password".
     - В поле "Username" укажите ваш GitHub username.
     - В поле "Password" укажите ваш GitHub Personal Access Token.
     - Нажмите "Add".
   - Выберите созданные Credentials в поле "Credentials".
   - В поле "Script Path" укажите путь к файлу пайплайна относительно корня репозитория (например, `Jenkinsfile`).

4. **Запуск пайплайна**:

   - Нажмите "Save".
   - Нажмите "Build Now", чтобы запустить пайплайн вручную.

#### 3. Проверка доступа к файлам/папкам

Если возникают проблемы с доступом к файлам или папкам на сервере, где установлен Jenkins, убедитесь, что у пользователя Jenkins есть необходимые права:

```sh
sudo chown -R jenkins:jenkins /path/to/your/files
sudo chmod -R 755 /path/to/your/files
```

#### 4. Дополнительные настройки

- **Установка плагинов**: В Jenkins выберите "Manage Jenkins" -> "Manage Plugins" и установите необходимые плагины, такие как "Pipeline", "Git", "SSH Agent" и другие.
- **Настройка безопасности**: В Jenkins выберите "Manage Jenkins" -> "Configure Global Security" и настройте безопасность вашего Jenkins-сервера.

### Пример `Jenkinsfile`

Пример простого `Jenkinsfile` для пайплайна:

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Building..."'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Testing..."'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying..."'
            }
        }
    }
}
```

### Заключение

Теперь ваш Jenkins настроен для автоматического запуска пайплайна из Git-репозитория. Jenkins будет опрашивать репозиторий каждые 5 минут и запускать пайплайн при обнаружении изменений. Убедитесь, что у вас есть необходимые права доступа к файлам и папкам на сервере, где установлен Jenkins.

# JenkinsTests
# jenkinsTests
