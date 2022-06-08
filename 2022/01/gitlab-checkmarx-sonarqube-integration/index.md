# GitLab Checkmarx SonarQube integration


## Purpose

<!-- wp:paragraph -->
<p><img src="https://dennys.files.wordpress.com/2022/01/image-2.png" alt=""></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":186,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://dennys.files.wordpress.com/2022/01/image-5.png"><img src="https://dennys.files.wordpress.com/2022/01/image-5.png?w=253" alt="" class="wp-image-186"/></a></figure>
<!-- /wp:image -->

When we commit code to&nbsp;<a href="https://gitlab.com/">GitLab</a>, we want</p>

1. [GitLab](https://gitlab.com/) sends the code to [Checkmarx](https://checkmarx.com/) to scan.
1. [GitLab](https://gitlab.com/) triggers [SonarQube](https://www.sonarqube.org/) to sacn.
1. [SonarQube](https://www.sonarqube.org/) integrate [Checkmarx](https://checkmarx.com/)'s report.

## Procedure
You can reference the official document: https://checkmarx.atlassian.net/wiki/spaces/SD/pages/169246832/SonarQube+Plugin+v8.5.0+and+up

1. Download the plugin from <a href="https://checkmarx.com/plugins/">here</a>, it only supports SonarQube LTS version (for now, it's 8.x)
1. Configurea Quality Gate/Profiles of SonarQube for Checkmarx's rules.
1. Use GitLab to trigger Checkmarx scan and record the project name of Checkmarx.
1. Configure Checkmarx data in SonarQube, you can reference <a href="https://checkmarx.atlassian.net/wiki/spaces/SD/pages/169345207/Configuring+a+Project+for+the+Checkmarx+SonarQube+Plugin+v8.5.0+and+up">here</a>.
1. Trigger GitLab CI again, you will see the following log in your SonarQube job

<!-- wp:code -->
<pre class="wp-block-code"><code>INFO: Sensor Import Checkmarx scan results to SonarQube &#91;checkmarx]
INFO: Retrieving Checkmarx scan results for current module &#91;Checkmarx plugin version: 2021.2.1]
INFO: Getting Checkmarx configuration data from sonar Database.
INFO: Resolving Cx setting: checkmarx.server.project_name
INFO: Forced authentication is enabled: Sonar credentials must be provided
INFO: Sonar server token is provided
INFO: Checkmarx credentials migration not needed
INFO: Sonar server token is provided
INFO: Resolving Cx setting: checkmarx.server.project_name
INFO: Forced authentication is enabled: Sonar credentials must be provided
INFO: Checkmarx server version &#91;9.2.0.41015]. Hotfix &#91;24].
INFO: Logging into the Checkmarx service.
INFO: Connecting to https://your.checkmarx.server/
INFO: Initializing Cx client &#91;2020.2.4.NO.SCA]
INFO: Checkmarx server version &#91;9.2.0.41015]. Hotfix &#91;24].
INFO: Logging into the Checkmarx service.
INFO: full team path: \CxServer\\Team1
INFO: preset name: All
INFO: ---------------------------------Get Last CxSAST Results:--------------------------------
INFO: Waiting for server to generate xml report. 4990 seconds left to timeout
INFO: Checkmarx High vulnerabilities: 3
INFO: Checkmarx New-High vulnerabilities: 0
INFO: Checkmarx Medium vulnerabilities: 23
INFO: Checkmarx New-Medium vulnerabilities: 1
INFO: Checkmarx Low vulnerabilities: 142
INFO: Checkmarx New-Low vulnerabilities: 7
INFO: Checkmarx scan link: https://your.checkmarx.server//CxWebClient/ViewerMain.aspx?scanId=1000157&amp;ProjectID=67</code></pre>
<!-- /wp:code -->

6. You can see the Checkmarx issues in SonarQube now.

