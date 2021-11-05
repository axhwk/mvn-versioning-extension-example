# Example to reproduce NullPointer

Reproduces: https://github.com/qoomon/maven-git-versioning-extension/issues/146

1. Run maven install with (this is only needed once):
    ```sh
    mvn install
    ```
    
2. Make sure no ```.git-versioned-pom.xml``` is present, e.g. 

    ```sh
    git clean -fd
    ```

3. Run maven test with:
    ```sh
    mvn -T 4 test -pl :moduleB,:moduleC -e -X
    ```

Running step 2 and 3 repeatedly should reproduce the error most of the times:
```
Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for moduleB 1.0-SNAPSHOT:
[INFO] 
[INFO] moduleB ............................................ SUCCESS [  3.114 s]
[INFO] moduleC ............................................ FAILURE [  0.436 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  4.748 s (Wall Clock)
[INFO] Finished at: 2021-11-05T08:18:29+01:00
[INFO] ------------------------------------------------------------------------
[ERROR] NullPointerException
java.lang.NullPointerException
    at me.qoomon.maven.gitversioning.MavenUtil.readXml (MavenUtil.java:83)
    at me.qoomon.maven.gitversioning.GitVersioningModelProcessor.writePomFile (GitVersioningModelProcessor.java:981)
    at me.qoomon.maven.gitversioning.GitVersioningModelProcessor.processModel (GitVersioningModelProcessor.java:272)
    at me.qoomon.maven.gitversioning.GitVersioningModelProcessor.read (GitVersioningModelProcessor.java:115)
    at org.apache.maven.model.building.DefaultModelBuilder.readModel (DefaultModelBuilder.java:552)
    at org.apache.maven.model.building.DefaultModelBuilder.readParentExternally (DefaultModelBuilder.java:1116)
    at org.apache.maven.model.building.DefaultModelBuilder.readParent (DefaultModelBuilder.java:846)
    at org.apache.maven.model.building.DefaultModelBuilder.build (DefaultModelBuilder.java:337)
    at org.apache.maven.repository.internal.DefaultArtifactDescriptorReader.loadPom (DefaultArtifactDescriptorReader.java:292)
    at org.apache.maven.repository.internal.DefaultArtifactDescriptorReader.readArtifactDescriptor (DefaultArtifactDescriptorReader.java:171)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.resolveCachedArtifactDescriptor (DefaultDependencyCollector.java:538)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.getArtifactDescriptorResult (DefaultDependencyCollector.java:523)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.processDependency (DefaultDependencyCollector.java:410)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.processDependency (DefaultDependencyCollector.java:362)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.process (DefaultDependencyCollector.java:349)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.collectDependencies (DefaultDependencyCollector.java:254)
    at org.eclipse.aether.internal.impl.DefaultRepositorySystem.collectDependencies (DefaultRepositorySystem.java:284)
    at org.apache.maven.project.DefaultProjectDependenciesResolver.resolve (DefaultProjectDependenciesResolver.java:169)
    at org.apache.maven.lifecycle.internal.LifecycleDependencyResolver.getDependencies (LifecycleDependencyResolver.java:243)
    at org.apache.maven.lifecycle.internal.LifecycleDependencyResolver.resolveProjectDependencies (LifecycleDependencyResolver.java:147)
    at org.apache.maven.lifecycle.internal.MojoExecutor.ensureDependenciesAreResolved (MojoExecutor.java:248)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:202)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:156)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:148)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)
    at org.apache.maven.lifecycle.internal.builder.multithreaded.MultiThreadedBuilder$1.call (MultiThreadedBuilder.java:190)
    at org.apache.maven.lifecycle.internal.builder.multithreaded.MultiThreadedBuilder$1.call (MultiThreadedBuilder.java:186)
    at java.util.concurrent.FutureTask.run (FutureTask.java:264)
    at java.util.concurrent.Executors$RunnableAdapter.call (Executors.java:515)
    at java.util.concurrent.FutureTask.run (FutureTask.java:264)
    at java.util.concurrent.ThreadPoolExecutor.runWorker (ThreadPoolExecutor.java:1128)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run (ThreadPoolExecutor.java:628)
    at java.lang.Thread.run (Thread.java:829)
[ERROR] java.lang.NullPointerException
java.util.concurrent.ExecutionException: java.lang.NullPointerException
    at java.util.concurrent.FutureTask.report (FutureTask.java:122)
    at java.util.concurrent.FutureTask.get (FutureTask.java:191)
    at org.apache.maven.lifecycle.internal.builder.multithreaded.MultiThreadedBuilder.multiThreadedProjectTaskSegmentBuild (MultiThreadedBuilder.java:145)
    at org.apache.maven.lifecycle.internal.builder.multithreaded.MultiThreadedBuilder.build (MultiThreadedBuilder.java:103)
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:128)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:305)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:192)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:105)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:957)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:289)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:193)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke0 (Native Method)
    at jdk.internal.reflect.NativeMethodAccessorImpl.invoke (NativeMethodAccessorImpl.java:62)
    at jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke (DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke (Method.java:566)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:282)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:225)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:406)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:347)
Caused by: java.lang.NullPointerException
    at me.qoomon.maven.gitversioning.MavenUtil.readXml (MavenUtil.java:83)
    at me.qoomon.maven.gitversioning.GitVersioningModelProcessor.writePomFile (GitVersioningModelProcessor.java:981)
    at me.qoomon.maven.gitversioning.GitVersioningModelProcessor.processModel (GitVersioningModelProcessor.java:272)
    at me.qoomon.maven.gitversioning.GitVersioningModelProcessor.read (GitVersioningModelProcessor.java:115)
    at org.apache.maven.model.building.DefaultModelBuilder.readModel (DefaultModelBuilder.java:552)
    at org.apache.maven.model.building.DefaultModelBuilder.readParentExternally (DefaultModelBuilder.java:1116)
    at org.apache.maven.model.building.DefaultModelBuilder.readParent (DefaultModelBuilder.java:846)
    at org.apache.maven.model.building.DefaultModelBuilder.build (DefaultModelBuilder.java:337)
    at org.apache.maven.repository.internal.DefaultArtifactDescriptorReader.loadPom (DefaultArtifactDescriptorReader.java:292)
    at org.apache.maven.repository.internal.DefaultArtifactDescriptorReader.readArtifactDescriptor (DefaultArtifactDescriptorReader.java:171)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.resolveCachedArtifactDescriptor (DefaultDependencyCollector.java:538)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.getArtifactDescriptorResult (DefaultDependencyCollector.java:523)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.processDependency (DefaultDependencyCollector.java:410)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.processDependency (DefaultDependencyCollector.java:362)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.process (DefaultDependencyCollector.java:349)
    at org.eclipse.aether.internal.impl.collect.DefaultDependencyCollector.collectDependencies (DefaultDependencyCollector.java:254)
    at org.eclipse.aether.internal.impl.DefaultRepositorySystem.collectDependencies (DefaultRepositorySystem.java:284)
    at org.apache.maven.project.DefaultProjectDependenciesResolver.resolve (DefaultProjectDependenciesResolver.java:169)
    at org.apache.maven.lifecycle.internal.LifecycleDependencyResolver.getDependencies (LifecycleDependencyResolver.java:243)
    at org.apache.maven.lifecycle.internal.LifecycleDependencyResolver.resolveProjectDependencies (LifecycleDependencyResolver.java:147)
    at org.apache.maven.lifecycle.internal.MojoExecutor.ensureDependenciesAreResolved (MojoExecutor.java:248)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:202)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:156)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:148)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:117)
    at org.apache.maven.lifecycle.internal.builder.multithreaded.MultiThreadedBuilder$1.call (MultiThreadedBuilder.java:190)
    at org.apache.maven.lifecycle.internal.builder.multithreaded.MultiThreadedBuilder$1.call (MultiThreadedBuilder.java:186)
    at java.util.concurrent.FutureTask.run (FutureTask.java:264)
    at java.util.concurrent.Executors$RunnableAdapter.call (Executors.java:515)
    at java.util.concurrent.FutureTask.run (FutureTask.java:264)
    at java.util.concurrent.ThreadPoolExecutor.runWorker (ThreadPoolExecutor.java:1128)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run (ThreadPoolExecutor.java:628)
    at java.lang.Thread.run (Thread.java:829)
[ERROR] 
```