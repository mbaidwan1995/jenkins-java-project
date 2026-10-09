# Jenkins Java pipeline and troubleshooting lab

**Author of this case study:** Manjit Baidwan | **Scope:** Personal learning project

I used a Java application reference repository to practise Jenkins setup and pipeline troubleshooting on a cloud VM. My lab notes record a successful build on attempt 24 after earlier failures, and identify a version mismatch that I resolved. This README makes that experience and the checked-in pipeline scope easier to review.

## Attribution

This repository is forked from [devopsbyraham/jenkins-java-project](https://github.com/devopsbyraham/jenkins-java-project). The Java application, Maven configuration and original Jenkinsfile are upstream work. My contribution documented here is the lab setup and troubleshooting recorded in my project notes, plus this portfolio write-up. Local setup changes are not all represented by commits in this fork.

## Checked-in pipeline

~~~text
Git checkout -> Maven compile -> Maven test -> Maven package -> S3 artifact upload
~~~

| Stage | Behavior in Jenkinsfile |
| --- | --- |
| code | Checks out the upstream Java repository |
| build | Runs mvn compile |
| test | Runs mvn test; the stage alone does not establish test coverage or a passing run |
| artifact | Runs mvn package |
| s3 | Uses the Jenkins S3 upload step with a configured bucket, region, profile and WAR path |
| deploy | Prints a message; no actual application deployment command is implemented |

Review [Jenkinsfile](Jenkinsfile), [pom.xml](pom.xml) and [src](src). Bucket, profile and artifact values are lab-specific and must be adapted to the intended environment. The current pipeline does not contain Docker image build/push steps, even though my notes discuss a Docker-oriented workflow.

## My lab setup

My project report records these activities:

- Launched a VM and connected using SSH.
- Installed Java and Jenkins and configured access to Jenkins on port 8080.
- Added a Jenkins pipeline, installed required plugins and attached an IAM role to the VM.
- Installed Tomcat as part of the exercise.
- Investigated repeated build failures and resolved a version mismatch.

Installing Tomcat does not itself prove that the application was deployed and reachable.

## Troubleshooting case

**Situation:** The Jenkins build failed repeatedly during the setup exercise.

**Issue recorded:** A version mismatch. My notes do not retain the exact components, versions or error message, so this README does not invent those details.

**Action recorded:** Corrected the mismatch and continued the build attempts.

**Result recorded:** Build 24 succeeded after 23 failed attempts, according to my lab report. The original Jenkins console log and successful stage view have not been attached here. The report does not show which stages ran in that successful build.

**Lesson:** Capture the first failing stage and full error, record tool/plugin versions, change one relevant setting and compare the next build output. A green build should be checked against the stages that actually ran and the artifact produced.

## Evidence to complete the case study

- Failed-build error excerpt and successful build-24 console output.
- Before/after versions and the exact configuration correction.
- Jenkins stage view and Maven test results, if tests executed.
- WAR artifact details and S3 upload confirmation, if that stage executed.
- An application response from Tomcat before claiming deployment success.

Screenshots and logs should be sanitized. Keep credentials in Jenkins/AWS credential mechanisms, and limit Jenkins access to the intended lab users.

## Relevant skills

Jenkins, Java build tooling, Maven, Linux VM setup, SSH, IAM roles, CI troubleshooting, artifact handling and technical documentation.

[Professional profile](https://www.linkedin.com/in/manjit-baidwan-a6a477406/)
