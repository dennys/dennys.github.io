# Use GitLab to do .NET 4.X CI/CD


## Precondition

You already have a GitLab ([GitLab Saas](http://gitlab.com/) or GitLab self managed). This document is for .NET 4.X, if you want to build .NET core or .NET 5/6/7/..., you can use docker to run GitLab Runner.

### Step 1 - Install Git client

Download and install [Git for Windows](https://git-scm.com/download/win). Portable version is ok, but because Git Runner is a Windows service, please add git.exe into PATH.

### Step 2 - prepare .NET build environment

Download and install [Visual Studio](https://visualstudio.microsoft.com/). (Please let me know if I can build a .NET build environment without Visual Studio, thanks.)

### Step 3 - Install GitLab Runner

1. Download [GitLab Runner](https://docs.gitlab.com/runner/install/windows.html)
1. Rename gitlab-runner-windows-amd64.exe to gitlab-runner.exe
1. Get the token to run GitLab Runner<img class="wp-image-135" style="width:500px;" src="https://dennys.files.wordpress.com/2021/12/1-1.png" alt="">
1. Generate GitLab Runner configuration file (config.toml)
    1. Run the command 
    
        gitlab-runner.exe register --url https://gitlab.com/ --registration-token $REGISTRATION_TOKEN

1. Modify config.toml
    1. Because Gitlab Runner doesn't suppoer Windows shell after version 13, you need to use PowerShell. If you use Power Shell, please open config.toml and rename 'pwsh' to 'powershell'. (If you use [Power Shell Core](https://github.com/PowerShell/PowerShell), you don't need to do anything)
    1. If you use a portable Git, you need to modify <code>$env:Path</code> of PowerShell.
    1. If you find your log is garbled (ex: your Windows server is not English version), please add "chcp 65001" to change the encoding to UTF-8.
1. Register GitLab Runner as a Windows service and start it.
    1. Run the command <code>gitlab-runner install</code> to install as a Windows service.
    1. Run the command <code>gitlab-runner start</code> to start the service.
1. config.toml sample =&gt; <mark style="background-color:#ff675f;" class="has-inline-color">remember, if you modify the file, please execute <code>gitlab-runner.exe restart</code> to restart GitLab runner.</mark>


<!-- wp:code -->
<pre class="wp-block-code"><code>concurrent = 1
check_interval = 0

&#91;session_server]
  session_timeout = 1800

&#91;&#91;runners]]
  name = "GitLab Runner 007"
  url = "https://gitlab.com/"
  token = "******************"
  executor = "shell"
  shell = "powershell"
  pre_clone_script = """
      chcp 65001
      $env:Path += ";C:\\Gitlab\\PortableGit\\cmd"
  """
  &#91;runners.custom_build_dir]
  &#91;runners.cache]
    &#91;runners.cache.s3]
    &#91;runners.cache.gcs]
    &#91;runners.cache.azure]</code></pre>
<!-- /wp:code -->


If you always see the Credential Helper Selector, please choose "no helper" and "Always use this from now on".

<figure class="wp-block-image size-large is-resized"><a href="https://dennys.files.wordpress.com/2021/12/image-4.png"><img src="https://dennys.files.wordpress.com/2021/12/image-4.png?w=386" alt="" class="wp-image-146" width="322" height="208"/></a></figure>

### Step 4 - Write .gitlab-ci.yml

1. Edit .gitlab-ci.yml, the following is a sample:

<!-- wp:code -->
<pre class="wp-block-code"><code>stages:
    - build
    - test

build:
    stage: build
    script:
        - "dotnet build"
    artifacts:
      paths:
        - .\test

test:
    stage: test
    script: 
        - "dotnet test"</code></pre>
<!-- /wp:code -->

### Step 5 - Start to build/test/deploy code (local machine)

Change directory to the location of .gitlab-ci.yml and execute this command

    gitlab-runner.exe exec shell build

### Step 6 - Start to test CI/CD (local machine)

If everything is ok, you can commit .gitlab-ci.yml and GitLab should run it automatically.

### SAST (Static Application Security Testing)

GitLab can check your source code for known vulnerabilities, unfortunately, it only support Linux container, Windows containers are not yet supported. (reference: https://docs.gitlab.com/ee/user/application_security/sast/)

