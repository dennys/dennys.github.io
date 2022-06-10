# GitLab Checkmarx integration


![GitLab](/images/software/logo/GitLab_logo.webp)
![Checkmarx](/images/software/logo/Checkmarx_logo.png)

## Purpose
When we commit code to [GitLab](https://gitlab.com/), we want GitLab sends the code to [Checkmarx](https://checkmarx.com/) to scan. (the next step is to integrate the scan result to SonarQube)

## Procedure

You can reference official document: https://checkmarx.atlassian.net/wiki/spaces/SD/pages/1929937052/GitLab+Integration</p>

1. Edit .gitlab-ci.yml

```
include: 'https://raw.githubusercontent.com/checkmarx-ltd/cx-flow/develop/templates/gitlab/v3/Checkmarx.gitlab-ci.yml'

variables:
    CX_PROJECT: ProjectXXX #The project name you want to show in Checkmarx
```

2. Edit GitLab CI/CD variables

Please remember to set ***GITLAB_URL*** and ***GITLAB_TOKEN***, then Checkmarx will create GitLab issue for Checkmarx issues.

<a href="https://dennys.files.wordpress.com/2022/01/image-4.png"><img src="https://dennys.files.wordpress.com/2022/01/image-4.png?w=1024" alt="" class="wp-image-181"/></a>

3. Trigger build - Checkmarx analysis will be triggered when you create a merge request or you commit to master stream directly.

Suggest to use pure English for CX_TEAM, if you use non English (ex: Chinese), you can check the log and find it cannot find the team.

```
2022-01-12 02:51:22.748  INFO 11 --- [main] c.c.s.s.CxService [x4DNMhOL] : Found team /CxServer/Team1 with ID 16
```

During the analysis, you can check the status from Checkmarx, it takes long time to analyze.

<a href="https://dennys.files.wordpress.com/2022/01/image-6.png"><img src="https://dennys.files.wordpress.com/2022/01/image-6.png?w=1024" alt="" class="wp-image-193"/></a>

After the analysis, you can check the result in Checkmarx.

You can also check it from GitLab, it generates 1 report in merge request and it will create issues by category. The following is the reqport of merge request.

<a href="https://dennys.files.wordpress.com/2022/01/image-7.png"><img src="https://dennys.files.wordpress.com/2022/01/image-7.png?w=939" alt="" class="wp-image-197"/></a>

In the above report, there are 5 issues, it will creates 5 issues by file/by category.

<a href="https://dennys.files.wordpress.com/2022/01/image-8.png"><img src="https://dennys.files.wordpress.com/2022/01/image-8.png?w=1010" alt="" class="wp-image-199"/></a>

In the issue, it shows the detail vulnerability information.

<a href="https://dennys.files.wordpress.com/2022/01/image-9.png"><img src="https://dennys.files.wordpress.com/2022/01/image-9.png?w=1024" alt="" class="wp-image-201"/></a>

