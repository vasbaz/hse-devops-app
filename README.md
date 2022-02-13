# CI/CD pipeline description

As an example, I chose the "Hello world" backend app in python. All it does is return json `{"message":"Hi! I'm a CI/CD
application"}` at the enpoint “/“ (local default is http://127.0.0.1:8000/).

As a framework, I use **FastAPI**. FastAPI is a very young, but already the 3rd most used backend
framework (https://www.jetbrains.com/ru-ru/lp/python-developers-survey-2020/). In ideology, it is similar to Flask,
which is also a python micro-framework for backend, but so far in 2nd place according to a survey from JetBrains. I
like FastAPI a lot. :)

The first step in my CI/CD is **testing**. The `pip install -r requirements.txt` command installs all the necessary
dependencies to run the application, and the `pytest --junitxml=report.xml` command runs the tests. As an example, in
the test folder there is one test that assumes that the app sends json like this: `{"message":"Hello! I am a CI/CD
application"}`. The flag `--junitxml=report.xml` is needed to tab appear with a test report in the gitlab
interface (https://docs.gitlab.com/ee/ci/unit_test_reports.html).

In the second step, I **build** the Docker image, having previously logged into the Google Cloud Platform
artifactory (https://cloud.google.com/artifact-registry). I use Google artifactory, because I will also deploy on the
Google Cloud Platform, and this thing cannot pull images from external artifactories.

Thirdly, I **deploy** the image on the Cloud Run service (https://cloud.google.com/run). This service is intended for
deploying single containers (for example, when k8s is redundant). To do this, in before_script, I need to log into the
service account using the Google Cloud SDK, and then run the `gcloud run deploy`
command (https://cloud.google.com/sdk/gcloud/reference/run).

For a wide range of issues, my telegram: @vasbaz