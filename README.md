# jenkins
# What is Jenkins?
- **Jenkins** is open-source automation server used primarily for **Continuous Integration(CI) and Continuous Delivery/Deployment(CD)** in software development.
- it helps developers to automate the process of building ,testing, deploying code and making software delivery faster and more reliable.
## Why Jenkins is used 
- it automates **repetitive ** task like building the code, running test and deploying to servers.
- it ensures new change in code doesn't break the existing  functionality through continuous testing.
- integrate with version control system like git and tools like docker,kubernetes, mavne and ansible.
## 🧱 Core Features of Jenkins 
| Feature                | Description                                                                    |
| ---------------------- | ------------------------------------------------------------------------------ |
| **Pipeline as Code**   | Define your CI/CD workflow using `Jenkinsfile` (written in Groovy)             |
| **Plugins**            | 1800+ plugins for integration with tools like GitHub, Docker, Slack, SonarQube |
| **Distributed Builds** | Supports master-agent architecture to scale builds across multiple machines    |
| **Easy Integration**   | Works with Java, Node.js, Python, .NET, etc.                                   |
| **Scheduling**         | Jobs can run on code push, pull request, daily/weekly schedule, or manually    |

## 🔄 How Jenkins Works
- developer push the code to the version control system(like git)
- jenkins detect the changes(using webhook or polling)
- jenkinss build the application( complies code, installs the dependencies)
- It runs automated tests.

- If all checks pass, Jenkins can deploy the code to staging/production.

- Sends notifications (e.g., via email, Slack) about the job result.
## 🛠️ Jenkins Use Cases
- Continuous Integration pipelines
- Automated testing
- Docker and Kubernetes deployment
- Infrastructure automation with Ansible or Terraform
- Code quality checks using SonarQube
 ## 🏗️ Jenkins Architecture
###  🔸 1. Master-Agent Architecture
- Jenkins Master:

   - Coordinates tasks.

   - Runs the web UI and schedules jobs.

- Jenkins Agent (formerly "slave"):

   - Executes build jobs.

   - Can be installed on different OS platforms or cloud instances (like AWS EC2, Docker, Kubernetes pods).

- 👉 Agents allow scalability and load distribution, which is especially useful in large teams or projects.

## 🧰 Jenkins Plugins
| Plugin Type              | Example Plugins              | Functionality                        |
| ------------------------ | ---------------------------- | ------------------------------------ |
| **Source Control**       | Git, GitHub, Bitbucket       | Pull code from repositories          |
| **Build Tools**          | Maven, Gradle, Ant           | Compile and build code               |
| **Testing**              | JUnit, Selenium, Cucumber    | Automate tests and visualize reports |
| **Deployment**           | SSH, AWS, Docker, Kubernetes | Automate deployment steps            |
| **Notifications**        | Slack, Email, Discord        | Alert build status                   |
| **Static Code Analysis** | SonarQube, Checkstyle        | Code quality checks                  |

## 📌 Benefits of Jenkins
- ✅ 100% Free and Open Source
- ✅ Supports all major languages and platforms
- ✅ Easily customizable workflows
- ✅ Scalable across large projects
- ✅ Integrates with any DevOps toolchain


