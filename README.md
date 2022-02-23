# DevOps Best Practices

This is a guideline of best practices that we can apply to our software projects.
DevOps best practices include subjects such as agile project management, shifting left with CI/CD, automation, monitoring, observability, continuous feedback, and others.
These tips are based on books, articles and professional experience.

## Table of Contents

1. [Identify tests that can be automated](#identify-tests-that-can-be-automated)
2. [Run the tests in parallel](#run-the-tests-in-parallel)
3. [Avoid false negatives and false positives](#avoid-false-negatives-and-false-positives)
4. [Check the test reports](#check-the-test-reports)
5. [Run the fastest tests early](#run-the-fastest-tests-early)
6. [Run the tests locally](#run-the-tests-locally)
7. [Do code commits frequently](#do-code-commits-frequently)
8. [Commit to the baseline every day](#commit-to-the-baseline-every-day)
9. [Have a central repository](#have-a-central-repository)
10. [Minimize the number of branches](#minimize-the-number-of-branches)
11. [Focus on documentation](#focus-on-documentation)
12. [Do incremental changes](#do-incremental-changes)
13. [Involving relevant stakeholders](#involving-relevant-stakeholders)
14. [Choose the right automation tools](#choose-the-right-automation-tools)
15. [Publish the results of the latest build](#publish-the-results-of-the-latest-build)
16. [Automate the build and deployment](#automate-the-build-and-deployment)
17. [Decouple releases from deployments](#decouple-releases-from-deployments)
18. [Make the pipeline the only way to deploy](#make-the-pipeline-the-only-way-to-deploy)
19. [Deploy the same way to all environments](#deploy-the-same-way-to-all-environments)
20. [Deploy into a production-like environment](#deploy-into-a-production-like-environment)
21. [Staging close to the production environment](#staging-close-to-the-production-environment)
22. [Build only once and promote the result](#build-only-once-and-promote-the-result)
23. [Release early, release often](#release-early-release-often)
24. [Emergency deployments and rollbacks](#emergency-deployments-and-rollbacks)
25. [Rollback with version control](#rollback-with-version-control)
26. [One-click migration](#one-click-migration)
27. [Keep the pipelines fast](#keep-the-pipelines-fast)
28. [Use Pipeline as Code](#use-pipeline-as-code)
29. [Do all work within a stage](#do-all-work-within-a-stage)
30. [Parallelize whenever possible](#parallelize-whenever-possible)
31. [Wrap the inputs in a timeout](#wrap-the-inputs-in-a-timeout)
32. [Avoid complex scripts in code pipeline](#avoid-complex-scripts-in-code-pipeline)
33. [Monitor the CI/CD pipelines](#monitor-the-cicd-pipelines)
34. [Smoke-Test the deployments](#smoke-test-the-deployments)
35. [Instant propagation](#instant-propagation)
36. [Stop the pipeline when it fails](#stop-the-pipeline-when-it-fails)
37. [Use Infrastructure as Code](#use-infrastructure-as-code)
38. [Test the infrastructure code](#test-the-infrastructure-code)
39. [Version control for all](#version-control-for-all)
40. [Do peer-review](#do-peer-review)
41. [Always have backups](#always-have-backups)
42. [Use GitOps](#use-gitops)
43. [Take a security first approach](#take-a-security-first-approach)
44. [Run scripts from a limited account](#run-scripts-from-a-limited-account)
45. [Use horizontal privilege escalation](#use-horizontal-privilege-escalation)
46. [Follow Principles of Chaos Engineering](#follow-principles-of-chaos-engineering)
47. [Practice redundancy](#practice-redundancy)
48. [Do application monitoring](#do-application-monitoring)
49. [Review metrics on a regular basis](#review-metrics-on-a-regular-basis)
50. [Have an alert system](#have-an-alert-system)

## Identify tests that can be automated

It is unlikely that 100% of the tests can be automated since there would at least be some tests where manual testing would be more effective when compared to automation tests.
Since test automation is core to CI/CD pipeline, realizing those test cases which can be automated is a crucial best practice for CI/CD.
We can automate tests that are executed on a frequent basis and tests that require knowledge and depend on a specific set of testers.

## Run the tests in parallel

Having automated testing already accelerates the entire process but the results would be much better if it is coupled with parallel testing.
We can have number of automation tests being executed, simultaneously, which can yield results much faster.
We cannot get the best throughput out of parallel testing if the tests are executed on one single machine.
Such a scenario would hog the critical resources on the machine e.g. CPU, GPU, etc.
which may slow down the execution of the other tests running on the machine.
Hence, the infrastructure on which the tests are executed matter a lot.

## Avoid false negatives and false positives

Sometimes, a system works fine fundamentally. However, automation scripts don't reflect the same.
They state otherwise and cause a false positive scenario.
Thus, it creates a situation of confusion and wastes time, effort, and resources.
Another scenario is that when the automation script gives the green signal and there is something wrong.
The system isn't working as it should, but the script declares otherwise.
Network issues can cause discrepancies in the test environment settings.
Leaving a system in a compromised state can cause catastrophic consequences in the long term.

## Check the test reports

After carrying out the tests, the tester must make a thorough test report.
If not carried out properly, the analysis can leave faults unattended and cause wastage of time, resources, and efforts.
Some tests succeed and some fail in automated testing.
Therefore, it is mandatory to examine test reports for faults and analyse the reason behind the failure of certain tests.
It is better to conduct the analysis manually to uncover genuine failures.
It is vital to unmask hidden problems and make sure that they don't get overlooked due to masking by other issues.

## Run the fastest tests early

While keeping the entire pipeline fast is a great general goal, parts of the test suite will inevitably be faster than others.
Discovering failures as early as possible is important to minimize the resources devoted to problematic builds.
To achieve this, prioritize and run the fastest tests first.
Save complex, long-running tests until after we have validated the build with smaller, quick-running tests.
Test prioritization usually means running the project's unit tests first since those tend to be quick, isolated, and component focused.
Afterwards, integration tests typically represent the next level of complexity and speed, and finally e2e tests, which often require some level of interaction.

## Run the tests locally

Developers should be encouraged to run as many tests as possible locally prior to committing to the repository and/or integration branch.
This makes it possible to detect certain problematic changes before they block other team members.
To ensure that developers can test effectively on their own, the test suite should be runnable with a single command that can be run from any environment.
The same command used by developers on their local machines should be used by the CI/CD system to kick off tests on code merged to the repository.

## Do code commits frequently

Developers begin the development based on the requirements mentioned in the specifications.
Once the implementation is complete, the developer performs a unit test on the code and fixes the issues encountered during this round of testing.
Once the local testing is complete, the developer will push the code to a code repository.
In most of the development environments, the code is normally pushed to a "feature branch" and once that is approved and tested, it can be pushed to the "master branch".
Hence, developers should be encouraged and motivated to push the code changes more frequently so that it is easy to keep track of changes.

## Commit to the baseline every day

By committing regularly, every committer can reduce the number of conflicting changes.
Checking in a week's worth of work runs the risk of conflicting with other features and can be very difficult to resolve.
Early, small conflicts in an area of the system cause team members to communicate about the change they are making.
Committing all changes at least once a day is generally considered part of the definition of [Continuous Integration](https://www.atlassian.com/continuous-delivery/continuous-integration).
However, no in-progress work should be committed into the master branch and do not commit to any branch if the build is broken.
In the worst case scenarios, the developer must regularly update his branch with the integration branch.

## Have a central repository

Maintaining the source code on central repository is of utmost importance and is considered one of the best practices for CI/CD pipeline.
So that the developers can keep their changes up to date with the latest source code available on the production server.
A revision/version control system is also important to track changes, identify differences, and maintaining an environment that eases the task to keep track of the application builds.

## Minimize the number of branches

One of the main principles of CI/CD is to integrate changes into the primary shared repository early and often.
This helps avoid costly integration problems down the line when multiple developers attempt to merge large, divergent, and conflicting changes into the master branch of the repository in preparation for release.
To take advantage of the benefits that CI provides, it is best to limit the number and scope of branches in the repository.
Most implementations suggest that developers commit directly to the master branch or merge changes from their local branches in at least once a day.

## Focus on documentation

Automation code is usually written, added, and updated by multiple individuals to a version control system.
In such an environment, proper documentation, comments, and consistent naming conventions are of the utmost importance.
It helps to organize the code of each developer and helps his colleagues to follow the entire code base.
If one coder leaves the team or wants to add new functionality by using existing code, they can debug, update, test and analyse results with greater results.
Even if there is a difference in the nature of the projects, we can still make use of some best practices from the past projects so that we can accelerate every phase of the current project.
Also, to avoid repeating the mistakes that were committed in other projects.

## Do incremental changes

Developers can follow a big-bang approach to develop a new feature.
This means that the developer will use the option where he will push the feature implementation at one shot.
Though the implementation job is done, there are issues with this approach.
The major drawback is that it becomes extremely difficult to isolate an issue when there is a problem with the implementation.
A wiser approach would be to break down the feature into different sub-features and make use of unique feature flags for each feature.
This technique not only helps in isolating potential issues but can be instrumental in making incremental feature-builds whenever the need arises.

## Involving relevant stakeholders

It becomes extremely critical to involve the right stakeholders in the automation planning, development, and execution.
Developers have a better understanding of the architecture, coding techniques, best practices in software development.
Experienced QA engineers may have a good understanding of test infrastructure, test frameworks, etc.
Involving developers, QA and IT Operations specialists is recommended for strong acceptance of the automation plan.

## Choose the right automation tools

There are several tools available for CI/CD, but we should choose the right tool based on our budget, requirements, and experience.
Some of the commonly used CI/CD tools are [Jenkins](https://www.jenkins.io/), [Travis CI](https://travis-ci.org/), [GitLab CI/CD](https://docs.gitlab.com/ee/ci/), [TeamCity](https://www.jetbrains.com/teamcity/), [CircleCI](https://circleci.com/), [Argo CD](https://argoproj.github.io/argo-cd/), [GoCD](https://www.gocd.org/), etc.
Other tools provide testing platforms that enables developers to test their websites and mobile applications across on-demand browsers, operating systems and mobile devices.
As an example, we have [Selenium Grid](https://www.selenium.dev/documentation/en/grid/), [BrowserStack](https://www.browserstack.com/), [Sauce Labs](https://saucelabs.com/), [LambdaTest](https://www.lambdatest.com/), [Applitools](https://applitools.com/), [Chromatic](https://www.chromatic.com/), [CrossBrowserTesting](https://crossbrowsertesting.com/), etc.
Tools like [Ansible](https://www.ansible.com/), [Chef](https://www.chef.io/) or [Puppet](https://puppet.com/) are used to manage application configuration and deployment in a way called Infrastructure as Code.

## Publish the results of the latest build

It should be easy to find out whether the build breaks.
All modern CI servers have the capability to display a dashboard containing the status of the builds and send email notifications when the build completes.
It is recommended that the emails are sent to the whole team when the build fails so that it can be fixed as soon as possible.
The various metrics that can be derived using the CI system by installing extensions can be made use of to improve not just the quality of the software, but also the quality of the development practices.

## Automate the build and deployment

In most situations, it is possible to write a script to build and deploy the application to a live test server that everyone can look at.
Automation of the build should include steps such as compiling the code, executing unit tests and integration tests.
They may also include several other tools as code quality checks, semantic checks, measuring technical debt, etc.
[Continuous Deployment](https://www.atlassian.com/continuous-delivery/continuous-deployment) is the next logical step after the process for CI/CD is in place and working well.
However, not all projects need automated deployment and not all commits result in a shippable product.

## Decouple releases from deployments

Adding a deployment stage before release for customers allows us to do smoke tests, or even more involved testing like [blue‑green deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html), [canary](https://martinfowler.com/bliki/CanaryRelease.html) or [A/B testing](https://en.wikipedia.org/wiki/A/B_testing). In a lot of organisations, release and deployment happen at the same time.
When deployment and release are highly coupled, deployment operations could be painful and dangerous.
It is recommend using the term Deployment when referring to the act of deploying a change to application components or infrastructure. The term Release should be used when a feature change is released to end users, with a business impact.
Using techniques such as [feature toggles](https://martinfowler.com/articles/feature-toggles.html) and [dark launches](https://martinfowler.com/bliki/DarkLaunching.html), we can deploy changes to production systems more frequently without releasing features.

## Make the pipeline the only way to deploy

Promoting code through our CI/CD pipelines requires each change to demonstrate that it adheres to our organization's codified standards and procedures.
Failures in a CI/CD pipeline are immediately visible and halt the advancement of the affected release to later stages of the cycle.
Frequently, teams start using their pipelines for deployment, but begin making exceptions when problems occur and there is pressure to resolve them quickly.
It is important to understand that the CI/CD system is a good tool to ensure that our changes are not introducing other bugs or further breaking the system.
We need to be disciplined to ensure that every change to our production environment goes through our pipeline.

## Deploy the same way to all environments

Use the same automated release mechanism for each environment, making sure the deployment process itself is not a source of potential issues.
We packed once to deploy the same code everywhere, and we need to deliver the same way.
By doing this, we'll avoid any surprises in production.
For this, we need to have production-like environment.
This doesn't necessarily mean that we'll have the same data everywhere.
It just means that if we have a load balancer in production because we need to be able to scale out/in, we also have to have one in development.

## Deploy into a production-like environment

Create a production-like or staging environment, identical to production, to validate changes before pushing them to production.
This will eliminate mismatches and last-minute surprises.
When we deploy something into staging, and it works, we can be reasonably assured that that version won't fail in production and cause an outage.
This environment is the final check of confidence.
Here it is important to have almost the same amount of data as we would in production.
This enables us to do load testing and test the scalability of the application in production.

## Staging close to the production environment

CI/CD pipelines promote changes through a series of test suites and deployment environments.
Reproducing the production environment as closely as possible in the testing environments helps ensure that the tests accurately reflect how the change would behave in production.
For improved effectiveness, the staging environment should ideally mirror the production environment.
This approach makes it easy for deploying working code from staging server to production server.
Some differences between staging and production are expected but keeping them manageable and making sure they are well-understood.

## Build only once and promote the result

If software requires a building, packaging, or bundling step, that step should be executed only once and the resulting output should be reused throughout the entire pipeline.
This guideline helps prevent problems that arise when software is compiled or packaged multiple times, allowing slight inconsistencies to be injected into the resulting artifacts.
CI systems should include a build process as the first step in the pipeline that creates and packages the software in a clean environment.
The resulting artifact should be versioned and uploaded to an artifact storage system to be pulled down by subsequent stages of the pipeline, ensuring that the build does not change as it progresses through the system.

## Release early, release often

[Release early, release often](https://en.wikipedia.org/wiki/Release_early,_release_often) is a software development philosophy that emphasizes the importance of early and frequent releases in creating a tight feedback loop between developers and testers or users, contrary to a feature-based release strategy.
Frequent releases are only possible if the software is in a release-ready state and we have tested it in a production-like environment.
It provides quick feedback, faster release cycles, less pressure and improves the user experience.

## Emergency deployments and rollbacks

As much as we want our code to work perfectly, there are situations when major bugs are discovered in a release after it is already deployed to an environment.
In such cases, rolling back the deployment is the best way to recover while we fix the problems.
As a best practice, always think of an easy way to roll back the changes if something goes wrong.
Normally, some organizations doing a rollback via redeploying the previous release or redoing the build from the stable code.
In some cases, we can make a hotfix.
A hotfix is essentially a pass to go directly into production. While it is best not to rely on them, when used in rare cases hotfixes can be a lifesaver.

## Rollback with version control

There are scenarios where the test team could come across some issues which were not observed in the previous software release.
A probable reason could be a side-effect of a fix that is pushed in the release software which is under test.
In such cases, the developer who pushed that fix should be able to roll-back his changes so that the release is not stalled, and he also gets some more time to re-look at his implementation.
Without version control system, such a seamless roll-back is not possible.
The roll-back is not limited to source code.
It can be extended to documents, presentations, flow diagrams, etc.

## One-click migration

The effort to move the code changes to the production environment can be greatly reduced if there is a one-click feature for migrating the code from one application environment to another.
A well-architected CI/CD pipeline should have one-click migration since it reduces the friction (in code migration) between different operations.
Opting for good cloud infrastructure that provides this feature and efficient and elegant usage of test automation could optimize the development and operation process.

## Keep the pipelines fast

Pipelines help shepherd changes through automated testing cycles, out to staging environments, and finally to production.
keeping our pipelines fast and dependable is incredibly important to not inhibit development velocity.
There are some straightforward steps we can take to improve speed, like scaling out our CI/CD infrastructure and optimizing tests.
Sometimes, paring down the test suite by removing tests with low value or with indeterminate conclusions is the smartest way to maintain the speed required by a heavily used pipeline.

## Use Pipeline as Code

[Pipelines as Code](https://about.gitlab.com/topics/ci-cd/pipeline-as-code/) emphasizes that the configuration of delivery pipelines that build, test and deploy our applications or infrastructure should be treated as code.
They should be placed under source control and modularized in reusable components with automated testing and deployment.
As organizations move to decentralized autonomous teams building microservices or micro frontends, the need for engineering practices in managing pipelines as code increases to keep building and deploying software consistent within the organization.
This need has given rise to delivery pipeline templates and tooling that enable a standardized way to build and deploy services and applications.

## Do all work within a stage

Stages contain a sequence of one or more stage directives.
The stages section is where the bulk of the "work" will be located.
Any non-setup work within our pipeline should occur within a stage block.
Stages are the logical segmentation of a pipeline. Separating work into stages allows separating our pipeline into distinct segments of work.
At a minimum, it is recommended that stages contain at least one stage directive for each discrete part of the CI/CD process, such as Build, Test, and Deploy.

## Parallelize whenever possible

Pipeline offers a straight-forward syntax for branching the pipeline into parallel steps.
Parallel builds speed up the build/test feedback loop, allowing the developers to be more productive, speed up deployments, so we can deploy more often.
Pipelines that build multiple artifacts could also build those artifacts in parallel.
When adding parallelization to the jobs, we should always monitor them to make sure it really speeds things up.
In some scenarios, heavy consumption of resources on the machine, for example, CPU, GPU, etc. may delay the execution of other operations running on the machine.

## Wrap the inputs in a timeout

CI/CD servers have a mechanism for timing out any given step of the pipeline.
As a best practice, we should always plan for timeouts around the inputs for healthy clean-up of the pipeline.
An example is when the pipeline waits for manual approval.
Wrapping the inputs in a timeout will allow them to be cleaned-up if approvals don't occur within a given time window.
Otherwise, the pipeline will run infinitely until the server times out.

## Avoid complex scripts in code pipeline

Long scripts generally indicate that they are doing too many things.
Small scripts are better to read and faster to understand the purpose.
Pipelines, as their complexity increases (the number of scripts, number of steps used, etc.), require more resources (CPU, memory, storage).
Think of pipeline as a tool to accomplish a build rather than the core of a build.
Therefore, it is critically important to reduce the number of scripts executed (this includes any methods called on classes imported in Pipelines).

## Monitor the CI/CD pipelines

Problems can and will occur in our pipeline. We may have a test which fails randomly, or have problems talking to a third-party service.
Sometimes individual build agents can have networks problems which cause builds to fail and build agents may deteriorate over time.
Having a broken CI/CD pipeline can potentially stall our development team.
We need to know when these occasional failures become significant enough to warrant action.

## Smoke-Test the deployments

Smoke testing allows us to quickly assess the status of an application by running a set of end-to-end tests targeted at checking the most important, or the most significant, user flows.
It should be run just after a fresh deploy and ideally at regular intervals after that.
It will give us the confidence that our application runs and passes basic diagnostics.
Smoke testing should be fast compared to end-to-end testing and the coverage is wide and shallow.

## Instant propagation

[Continuous Deployment](https://www.atlassian.com/continuous-delivery/continuous-deployment) must be an ongoing process of building, testing, release, and deployment.
Every check-in to a source control repository should trigger compiling, packaging, and deployment by a build server.
This, in turn, should automatically initiate certain predefined testing until the software can either be marked as releasable or returned to development.
[Continuous Integration](https://www.atlassian.com/continuous-delivery/continuous-integration) tools ensure that cascade of actions along the pipeline is consistent, automatic, and instant.
This means that the first stage should be triggered upon every check-in, and each stage should trigger the next one immediately upon successful completion.

## Stop the pipeline when it fails

When a stage in the pipeline fails, we should automatically stop the process. Fix whatever broke and start again from scratch before doing anything else.
This practice avoids cascading errors because subsequent steps will not fail. CI/CD servers interrupt job execution by default.

## Use Infrastructure as Code

[Infrastructure as Code](https://en.wikipedia.org/wiki/Infrastructure_as_code) (IaC) is a practice in which infrastructure is provisioned and managed using code and software development techniques, such as version control and continuous integration.
Without IaC, teams must maintain the settings of individual deployment environments.
Over time, each environment becomes a snowflake, that is, a unique configuration that cannot be reproduced automatically.
Inconsistency among environments leads to issues during deployments.
IaC enables DevOps teams to test applications in production-like environments early in the development cycle.
IaC avoids manual configuration of environments and enforce consistency by representing the desired state of the environments via code.

## Test the infrastructure code

Always test the infrastructure code in a non-production environment.
Companies have developed different pipelines to deploy code, and those usually include environments such as dev, test, staging, and production.
Use the same pipeline to test the infrastructure code.
Staged deployments can help us detect a problem before it is too late.
A staged deployment will allow us to test the new configuration on our actual load without risking downtime in case something was wrong.

## Version control for all

Everything from source code and configuration to infrastructure and the database should be version controlled.
Most of these techniques are better known as X as code, such as Pipeline as Code, Infrastructure as Code, and so forth.
Every change a developer makes must be documented in the source control repository or it is not included in the CI/CD process.
If a developer copies an artifact generated locally to a test environment, the next deployment will override this out-of-process change.
That's why the primary golden rule for effective Continuous Deployment is to version-control everything.
What works well for code also will preserve the integrity of configuration, scripts, databases, website html and even documentation.

## Do peer-review

[Peer-review](https://en.wikipedia.org/wiki/Peer_review) is the evaluation of work by one or more people with similar competencies as the producers of the work (peers).
The automation code can be critical, so we should always peer-review the automation code, whether we do it for our application code or not.

## Always have backups

Backup is crucial for system protection. Backups will not prevent us from nuking our machine. They will only make the restore process possible.
A regular data backup, preferably daily or weekly, saves the important files from inevitable data loss situations due to common events such as system crash, malware infection, hard drive corruption and failure, etc.

## Use GitOps

[GitOps](https://www.gitops.tech/) is a way of implementing [Continuous Deployment](https://www.atlassian.com/continuous-delivery/continuous-deployment) for cloud native applications.
It focuses on a developer-centric experience when operating infrastructure, by using tools developers are already familiar with, including Git and Continuous Deployment tools.
GitOps works by using Git as a source of truth for declarative infrastructure and applications.

## Take a security first approach

From an operational security standpoint, the CI/CD system represents some of the most critical infrastructure to protect.
We should consider isolating the CI/CD systems, placing them in secure internal networks.
VPNs, strong two-factor authentication and identity and access management systems will help us enforce "the principle of least privilege" and restrict exposure to threats.
[SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux) is a security kernel module that is available on all Linux distributions.
It allows us to limit users and process powers in a very granular way.

## Run scripts from a limited account

Using a limited user for all scripts, and escalating privileges only for commands that need higher privileges, will help prevent catastrophic events.
The principle of least privilege, when applied to privileged accounts, restricts the rights and access of a user to the minimal amount to perform their role.
Users should only be allowed to do what is appropriate for their job.

## Use horizontal privilege escalation

[Privilege escalation](https://en.wikipedia.org/wiki/Privilege_escalation) is the act of exploiting a bug, design flaw or configuration oversight in an operating system, or software application to gain elevated access to resources that are normally protected from an application or user.
For example, the `sudo` command supports the `-u` parameter that will allow us to specify a user that we want to impersonate.
If we must change a file that is owned by another user, we should escalate to that user.

## Follow Principles of Chaos Engineering

[Chaos Engineering](https://principlesofchaos.org/) is the idea that modern distributed software systems are prone to experiencing random, turbulent conditions and, therefore, such systems should be designed to withstand unexpected problems and weaknesses in production environments.
Before taking on chaos engineering, it's important to get the people, processes and technology to a somewhat stable state.
We should have a proactive system for incident detection and response, and we shouldn't be spending all of the time reacting to incidents in production.

## Practice redundancy

Redundancy is the duplication of critical components or functions of a system with the intention of increasing reliability of the system, usually in the form of a backup or fail-safe.
A redundant system will provide failover or load balancing support to protect a live system in the event of an unexpected failure.
In the case of power, mechanical, or software failure, a redundant system will have a duplicate component or platform to fall back to.
For example, if we have multiple data centers, we must place them in different locations.

## Do application monitoring

It can provide request and response information, database connection information, remote profiling and tracing for slow spots, and other metrics related to the health of the services.
Logs are generated all the time and provide more information than metrics.
The first key to good logging is to make our logs useful.
To do that, only log actionable events that machines, or humans can use.

## Review metrics on a regular basis

The thresholds we establish might be too high or too low.
If we're receiving too many alerts or we aren't notified when a legitimate issue occurs, it's time to review the threshold settings.
There are countless metrics that we can choose to monitor, and it can be overwhelming to separate the signal from the noise coming in.
The [RED Method](https://grafana.com/blog/2018/08/02/the-red-method-how-to-instrument-your-services/) provides a general framework for monitoring the health of a request-based service via three metrics: Rate, Errors and Duration.

## Have an alert system

Alerts allow us to identify problems in the system moments after they occur.
By quickly identifying unintended changes to the system, we can minimize service disruptions.
Alerts consists in alert rules and notification channel.
We should always aim to achieve the fastest and most efficient resolution for each type of alert we set up.
This is tied in with prioritization and should consider escalation.

## Bibliography

- [5 Common Reasons for Automation Tests Failure](https://www.browserstack.com/guide/reasons-for-automation-failure)
- [7 Best Practices for Continuous Delivery Success](https://devops.com/7-best-practices-continuous-delivery-success/)
- [8 Best Practices for Successful Implementation of DevOps in Your Enterprise](https://dev.to/credencys/8-best-practices-for-successful-implementation-of-devops-in-your-enterprise-2k93)
- [16 CI/CD Best Practices To Speed Up Test Automation](https://www.lambdatest.com/blog/16-best-practices-of-ci-cd-pipeline-to-speed-test-automation/)
- [22 Reasons Why Test Automation Fails For Your Web Application](https://dzone.com/articles/22-reasons-why-test-automation-fails-for-your-web)
- [An Introduction to CI/CD Best Practices](https://www.digitalocean.com/community/tutorials/an-introduction-to-ci-cd-best-practices)
- [An Introduction to Metrics, Monitoring, and Alerting](https://www.digitalocean.com/community/tutorials/an-introduction-to-metrics-monitoring-and-alerting)
- [Best Practices for Continuous Delivery](https://dzone.com/articles/best-practices-for-continuous-delivery)
- [Continuous Delivery Pipeline](https://www.scaledagileframework.com/continuous-delivery-pipeline/)
- [Continuous Testing Using Shift Left Testing Approach](https://www.lambdatest.com/blog/continuous-testing-using-shift-left-testing-approach/)
- [Continuous Integration Tools](https://www.atlassian.com/continuous-delivery/continuous-integration/tools)
- [Continuous Delivery Pipeline 101](https://www.atlassian.com/continuous-delivery/pipeline)
- [CI vs CD vs CD — What Are The Key Differences?](https://medium.com/swlh/ci-vs-cd-vs-cd-e102c6dd88eb)
- [CI/CD concepts](https://docs.gitlab.com/ee/ci/introduction/)
- [DevOps Best Practices](https://airbrake.io/blog/devops/devops-best-practices)
- [What is Continuous Testing?](https://www.tricentis.com/products/what-is-continuous-testing/)
- [What is Continuous Monitoring?](https://www.whizlabs.com/blog/continuous-monitoring/)
