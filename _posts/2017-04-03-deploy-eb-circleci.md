---
layout: post
title: CircleCI config to deploy to AWS Beanstalk
date: 2017-04-03 00:00:00 -07:00
comments: true
---

As part of your build deployment process you can push your current code to AWS Elastic Beanstalk.

Before we start, we'll need a few steps where we make sure CircelCi has the credentials to push to Elastic Beanstalk,

* Copy your beanstalk ssh key to a string literal: `cat ~/.ssh/eb.key | sed s/$/\\\\n/ | tr -d '\n'`
* Paste the resulting output as an environment variable in CircleCI project settings (https://circleci.com/gh/my/repo/edit#env-vars). Ex: `EB_KEY=<output of previous command goes here>`

Now we can use that environment variable after the build completes to deploy to EB. Add the following snippets to your `circle.yml` file

* First we'll need to install AWS EB cli tools as part of the test instance setup.
{% highlight yaml %}
machine:
  pre:
    - sudo -H pip install awsebcli
{% endhighlight %}
* Under deployment, I first restrict the deploy commands to a specific branch.
* Then we use the environment variable we created earlier to create a key file which the aws eb cli tools will look for to authenticate.
* Then we just deploy the current code using the top commit SHA as the version label. One could possibly even use the tag or a custom label.
{% highlight yaml %}
deployment:
  production:
    branch: master
    commands:
      - echo $EB_KEY | sed 's/\\n/\n/g' > ~/.ssh/eb.pem
      - cd ~/$CIRCLE_PROJECT_REPONAME
      - eb use my-production-environment
      - eb deploy -l $CIRCLE_SHA1 --timeout 15
{% endhighlight %}
