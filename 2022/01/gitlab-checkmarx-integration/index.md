# GitLab Checkmarx integration


<figure class="wp-block-image size-large"><a href="https://dennys.files.wordpress.com/2022/01/image.png"><img src="https://dennys.files.wordpress.com/2022/01/image.png?w=273" alt="" class="wp-image-170"/></a><figcaption><img src="https://dennys.files.wordpress.com/2022/01/image-2.png" alt=""></figcaption></figure>

## Purpose
When we commit code to [GitLab](https://gitlab.com/), we want GitLab sends the code to [Checkmarx] (https://checkmarx.com/) to scan. (the next step is to integrate the scan result to SonarQube)

## Procedure

You can reference official document: https://checkmarx.atlassian.net/wiki/spaces/SD/pages/1929937052/GitLab+Integration</p>

1. Edit .gitlab-ci.yml

<!-- wp:code -->
<pre class="wp-block-code"><code>include: 'https://raw.githubusercontent.com/checkmarx-ltd/cx-flow/develop/templates/gitlab/v3/Checkmarx.gitlab-ci.yml'

variables:
    CX_PROJECT: ProjectXXX #The project name you want to show in Checkmarx
</code></pre>
<!-- /wp:code -->

2. Edit GitLab CI/CD variables

Please remember to set GITLAB_URL and GITLAB_TOKEN, then Checkmarx will create GitLab issue for Checkmarx issues.

<!-- wp:image {"id":181,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://dennys.files.wordpress.com/2022/01/image-4.png"><img src="https://dennys.files.wordpress.com/2022/01/image-4.png?w=1024" alt="" class="wp-image-181"/></a></figure>
<!-- /wp:image -->

3. Trigger build - Checkmarx analysis will be triggered when you create a merge request or you commit to master stream directly.

Suggest to use pure English for CX_TEAM, if you use non English (ex: Chinese), you can check the log and find it cannot find the team.

    2022-01-12 02:51:22.748  INFO 11 --- [main] c.c.s.s.CxService [x4DNMhOL] : Found team /CxServer/Team1 with ID 16

During the analysis, you can check the status from Checkmarx, it takes long time to analyze.

<!-- wp:image {"id":193,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://dennys.files.wordpress.com/2022/01/image-6.png"><img src="https://dennys.files.wordpress.com/2022/01/image-6.png?w=1024" alt="" class="wp-image-193"/></a></figure>
<!-- /wp:image -->

After the analysis, you can check the result in Checkmarx.

You can also check it from GitLab, it generates 1 report in merge request and it will create issues by category. The following is the reqport of merge request.

<!-- wp:image {"id":197,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://dennys.files.wordpress.com/2022/01/image-7.png"><img src="https://dennys.files.wordpress.com/2022/01/image-7.png?w=939" alt="" class="wp-image-197"/></a></figure>
<!-- /wp:image -->

In the above report, there are 5 issues, it will creates 5 issues by file/by category.

<!-- wp:image {"id":199,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://dennys.files.wordpress.com/2022/01/image-8.png"><img src="https://dennys.files.wordpress.com/2022/01/image-8.png?w=1010" alt="" class="wp-image-199"/></a></figure>
<!-- /wp:image -->

In the issue, it shows the detail vulnerability information.

<!-- wp:image {"id":201,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://dennys.files.wordpress.com/2022/01/image-9.png"><img src="https://dennys.files.wordpress.com/2022/01/image-9.png?w=1024" alt="" class="wp-image-201"/></a></figure>
<!-- /wp:image -->
