
# Self Reference how-to for Maven
### Lukeeff
##### Source: http://tutorials.jenkov.com/maven/maven-tutorial.html

 <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    <!-- Version model of POM being used. 4.0.0 matches Maven 2 and 3  -->
    <modelVersion>4.0.0</modelVersion>

      
        * Group ID contains the unique ID for organization/project
        *
        * Artifact ID contains the name of the project. Also used
        *   as part of the name of the jar file produced when building
        *   the project. It is also the name of the subdirectory under
        *   the group ID directory in the Maven repository.
        *
        * Version ID contains the version number of the project. The
        *   version number is used as a name for a subdirectory under
        *   the artifact ID directory. It is also used as part of the
        *   name of the artifact built.
        *
        * ex: MAVEN_REPO/io.github.lukeeff/MavenLearning/1-0-SNAPSHOT/MavenLearning.jar
     
    <groupId>io.github.lukeeff</groupId>
    <artifactId>MavenLearning</artifactId>
    <version> 1.0-SNAPSHOT </version> 


   ### Dependencies / General info
        
        * Maven downloads each dependency that is needed and puts them in your local repository.
        *   If those libraries have libraries, then those other libraries are also downloaded to
        *   to your local Maven repository.
        *
        * Dependency elements are described by a few thing that describe an external library. Tags
        *   that were not defined in this scope were already defined and do the same thing as the
        *   prior definition.
        *
        * When the POM file is executed by Maven, the dependencies will be downloaded from a
        *   central Maven repository and injected into your local repository if they are missing.
        *
        * Sometimes a provided dependency is not available in the central Maven repository, which
        *   can be handled by downloading the repository yourself and putting it into the local Maven
        *   repository. When this is the case, it needs to follow the previously defined syntax in order
        *   for mapping to a directory to be possible (group id, version id, artifact id...) except for
        *   replacing the dots with slashes.
        *
        * ex: Maven_REPOSITORY_ROOT/io/github/lukeeff/MavenLearning/1-0-SNAPSHOT
        *
        * Snapshot represents a version under development. Instead of
        *   constantly updating the version numbers to get the latest version,
        *   you can just depend on a snapshot version of a project. Snapshot
        *   versions are always downloaded into the repo for every build even when
        *   a matching snapshot is already located in your local repository.
        *
        * Transitive dependencies are when your project depends on a dependency that
        *   depends on another dependency. When that's the case, it has a transitive dependency on
        *   the other dependency.
        *
   ##### Excluding Dependencies
        
        * Primary usage for external dependencies is related to dependency clashing. Occasionally direct dependencies
        *   of a project clash with transistive directories and when this is the case, you can simply exclude the
        *   unwanted dependency (the legacy version)
        
   ##### An example of dependency exclusion 
        
           <dependency>
               <groupId>example.jaxrs</groupId>
             <artifactId>JAX-RS-TOOLKIT</artifactId>
             <version>1.0</version>
             <scope>compile</scope>
             <exclusions>
               <exclusion>
                 <groupId>com.fasterxml.jackson.core</groupId>
                 <artifactId>jackson-core</artifactId>
               </exclusion>
             </exclusions>
           </dependency>
        



   ### External Dependencies
        * An external dependency in Maven is a dependency (JAR file) which is not located in a Maven
        *   repository (not local, remote, or central) It could be located in the local disk, such as
        *   in the lib directory of a webapp or unaffiliated folder or something. The word external refers
        *   to being external to the Maven repository system, not just external to the project. Most dependencies
        *   are external to the project, but few are external to the repository system. (not located in a repository)
        *
        *   > An example of an external dependency <
        *
        *   <dependency>
        *        <groupId>theDependency</groupId>
        *        <artifactId>theDependency</artifactId>
        *        <scope>system</scope>
        *        <version>1.0</version>
        *        <systemPath>${basedir}\war\WEB-INF\lib\theDependency.jar</systemPath>
        *   </dependency>
        *
        * * ${basedir} is a pointer to the directory where the POM is located.
        

    <dependencies>

        <!--This adds the Spigot API artifact to the build -->
        <dependency>
            <groupId>org.spigotmc</groupId>
            <artifactId>spigot</artifactId>
            <version>1.15.2-R0.1-SNAPSHOT</version>
            <scope>provided</scope>
        </dependency>

    </dependencies>




   ### Maven Repositories
        
        * Maven Repositories are directories of packaged JAR files with extra meta data. The meta data are POM
        *   files describing the projects each packaged jar file belongs to, including what external dependencies
        *   each packaged JAR has. This is the meta data that enables Maven to download dependencies of your
        *   dependencies recursively until all dependencies in the tree are downloaded and put into your
        *   local repository.
        
   #### Repository Types 
        *
        * Local Repository: A local repository on the developer's computer. Contains all dependencies Maven downloads.
        *   A single Maven repository is typically used for several different projects and only needs to download the
        *   dependencies once even if several projects depend on them. Your own projects can be built and installed in
        *   your local repository by simply using the mvn install command. After that other projects can use the
        *   packaged JAR files of your own projects as external dependencies by specifying them as dependencies from
        *   their respective POM.xml file. The location of the local repositories directory by default is the
        *   user-home/.m2/repositories directory and settings can be found there.
        *
        *   > Specifying another location for a local repository <
        *   <settings>
        *    <localRepository>
        *        d:\data\java\products\maven\repository
        *    </localRepository>
        *   </settings>
        *
        * Central Repository: The central Maven repository is a repository provided by the Maven community. By default,
        *   Maven looks here for any dependencies needed but not found in your local repository. Maven will download
        *   these dependencies into the local repository and no special configuration is needed to access the central
        *   repository.
        *
        * Remote Repository: A remote repository is a repository on a web server from which Maven can download
        *   dependencies, just like the central repository. A remote repository can be located anywhere on the
        *   internet or inside a local network. Remote repositories are often used for hosting projects internal to
        *   an organization which are shared by multiple projects. Example being a security project being used across
        *   multiple internal projects. It shouldn't be accessible to the outside world and thus not be hosted in the
        *   public, aka central Maven repository. Dependencies found in a remote repository are also downloaded and
        *   put into your local repository by Maven.
        *
        * > Configuring a remote repository in the POM file <
        *
        * Example:
        
           <repositories>
              <repository>
                  <id>jenkov.code</id>
                  <url>http://maven.jenkov.com/maven2/lib</url>
              </repository>
           </repositories>
        
 
 
 
   ### Maven Build Life Cycles:
        *   When Maven builds a software project it follows a build cycle that is divided into build phases and those
        *   build phases are divided into build goals. Maven has 3 built-in build cycles: Default, Clean, and Site.
        *   Each cycle takes care of a different aspect of building a software project, therefore each of these cycles
        *   are executed independently of each other, but they have to be executed in sequence, separately from one
        *   another as if you had executed two separate Maven commands.
        *
        * Default life cycle: Handles everything related to compiling and packaging a project.
        *
        * Clean life cycleL Handles everything related to removing temporary files from the output directory,
        *   including generated source files, compiled classes, previous jar files, etc.
        *
        * Site life cycle: Handles everything related to generating documentation for your project. Site can even
        *   generate a complete website with documentation for your project.
        *
        * Phases: Each build cycle is divided into a sequence of phases and those phases are subdivided into goals.
        * Therefore, the total build process is simply a sequence of build life cycles, build phases, and goals.
        *
        * You could execute either a whole build cycle, like clean or site, a build phase like install, which is part
        *   of the default build life cycle or a build goal life dependency:copy-dependencies. The default life
        *   cycle cannot be executed directly; a build phase or goal must be specified inside the default life cycle.
        *
        * When a build phase is executed, all build phases before that build phase in this standard phase sequence are
        *   executed, therefore executing the install build phase actually means executing all build phases before the
        *   install phase, then executed the install phase afterwards. The default life cycle is the one of most
        *   interest because that is what builds the code. Since you cannot execute the default life cycle directly,
        *   you need to execute a build phase or goal from the default life cycle.
        *
        * The default life cycle has an extensive sequence of phases and goals, with these being the most commonly used
        *   build phases:
        *
        *   Validate: Validates a project is correct and all necessary information is available. Also ensures that
        *               dependencies are downloaded.
        *
        *   Compile: Compiles the source code of the project.
        *
        *   Test: Runs the tests against the compiled source code using a suitable unit testing framework. These tests
        *           should not require the code to be packaged for deployed.
        *
        *   Package: Packs the compiled code in its distributable format, such as JAR.
        *
        *   Install: Install the package into the local repository, for use a a dependency in other projects locally.
        *
        *   Deploy: Copies the final package to the remote repository for sharing with other developers and projects.
        *
        * -> Maven plugins can also be created to meet the needs of any given project <-
        *
        * Build Goals: The finest steps in the Maven build process. Can be bound to one or more build phases or to
        *   none at all. If a goal is not build to any build phase, you cna only execute it by passing the name of the
        *   goal to the mvn command. If a goal is bound to multiple build phases, that goal will get executed during
        *   each of the build phases it has been bound to.

  



  ### Maven Build Profiles 

        
        * Maven build profiles allow you to build your own project using different configurations. Instead of creating
        *   two separate POM files, you can just specify a profile with the different build configuration and build
        *   your project with this build configuration when needed.
        *
        * Maven build profiles are specified inside of the POM file, inside of the profile's element. Each build
        *   profile is nested inside a profile element. 
        *   
        *  Example:
        
           <project xmlns="http://maven.apache.org/POM/4.0.0"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
               http://maven.apache.org/xsd/maven-4.0.0.xsd">
              <modelVersion>4.0.0</modelVersion>
        
             <groupId>com.jenkov.crawler</groupId>
             <artifactId>java-web-crawler</artifactId>
             <version>1.0.0</version>
              <profiles>
                   <profile>
                      <id>test</id>
                      <activation>...</activation>
                      <build>...</build>
                     <modules>...</modules>
                      <repositories>...</repositories>
                      <pluginRepositories>...</pluginRepositories>
                      <dependencies>...</dependencies>
                      <reporting>...</reporting>
                      <dependencyManagement>...</dependencyManagement>
                      <distributionManagement>...</distributionManagement>
                 </profile>
             </profiles>
        
            </project>

        * As you can see, a build profile describes what changes should be made to the POM file when executing
        *   under that build profile. This could be changing the application's configuration file to use etc.
        *   The elements inside of the profile element will override the values of the elements with the name name
        *   further up the POM.
        *
        * Inside of a profile element, there is an activation element. The activation element describes the condition
        *   that triggers the build profile to be used. One way to choose the profile to be executed is in the
        *   settings.xml file, which is where you can set the active profile. Another way is add -P profile-name to
        *   the Maven command line.
        *
  ### Maven Plugins 
        
        * Maven plugins give you the ability to add your own actions to the build process, which can be done by
        *   creating a simple java class that extends a special Maven class and then create a POM file for the
        *   project. The plugin should be located in its own project.
        
  

