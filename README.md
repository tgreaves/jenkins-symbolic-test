jenkins-symbolic-test
=====================

This repository consists of a 'bad' symbolic link, in that the file it links to does not exist.

That's not a problem for Git, Subversion et al.  However, Jenkins chokes on it when it comes to 'Archive' functionality (trace below).

    Started by user anonymous
    Building in workspace /var/lib/jenkins/jobs/symbolic test/workspace
    Updating svn://localhost/svn_symbolic
    At revision 2
    no change for svn://localhost/svn_symbolic since the previous build
    Archiving artifacts
    ERROR: Failed to archive artifacts: **/**
    hudson.util.IOException2: Failed to copy /var/lib/jenkins/jobs/symbolic test/workspace/**/** to /var/lib/jenkins/jobs/symbolic test/builds/2012-07-15_13-46-43/archive
        at hudson.FilePath$34.invoke(FilePath.java:1731)
        at hudson.FilePath$34.invoke(FilePath.java:1698)
        at hudson.FilePath.act(FilePath.java:842)
        at hudson.FilePath.act(FilePath.java:824)
        at hudson.FilePath.copyRecursiveTo(FilePath.java:1698)
        at hudson.tasks.ArtifactArchiver.perform(ArtifactArchiver.java:116)
        at hudson.tasks.BuildStepMonitor$1.perform(BuildStepMonitor.java:19)
        at hudson.model.AbstractBuild$AbstractBuildExecution.perform(AbstractBuild.java:717)
        at hudson.model.AbstractBuild$AbstractBuildExecution.performAllBuildSteps(AbstractBuild.java:692)
        at hudson.model.Build$BuildExecution.post2(Build.java:183)
        at hudson.model.AbstractBuild$AbstractBuildExecution.post(AbstractBuild.java:639)
        at hudson.model.Run.execute(Run.java:1513)
        at hudson.model.FreeStyleBuild.run(FreeStyleBuild.java:46)
        at hudson.model.ResourceController.execute(ResourceController.java:88)
        at hudson.model.Executor.run(Executor.java:236)
    Caused by: Failed to copy /var/lib/jenkins/jobs/symbolic test/workspace/bad_symbolic_link.link to /var/lib/jenkins/jobs/symbolic test/builds/2012-07-15_13-46-43/archive/bad_symbolic_link.link due to java.io.FileNotFoundException /var/lib/jenkins/jobs/symbolic test/workspace/bad_symbolic_link.link (No such file or directory)
        at org.apache.tools.ant.taskdefs.Copy.doFileOperations(Copy.java:914)
        at hudson.FilePath$34$1CopyImpl.doFileOperations(FilePath.java:1714)
        at org.apache.tools.ant.taskdefs.Copy.execute(Copy.java:567)
        at hudson.FilePath$34.invoke(FilePath.java:1728)
        ... 14 more
    Caused by: java.io.FileNotFoundException: /var/lib/jenkins/jobs/symbolic test/workspace/bad_symbolic_link.link (No such file or directory)
        at java.io.FileInputStream.open(Native Method)
        at java.io.FileInputStream.<init>(FileInputStream.java:137)
        at org.apache.tools.ant.util.ResourceUtils.copyResource(ResourceUtils.java:522)
        at org.apache.tools.ant.util.FileUtils.copyFile(FileUtils.java:559)
        at org.apache.tools.ant.taskdefs.Copy.doFileOperations(Copy.java:899)
        ... 17 more
    Finished: SUCCESS

References:
-----------

* https://issues.jenkins-ci.org/browse/JENKINS-5993
* https://issues.jenkins-ci.org/browse/JENKINS-5597
