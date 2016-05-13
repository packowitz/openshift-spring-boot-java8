# openshift-spring-boot-java8
This is an example how to setup an openshift application using Java8 and Spring Boot

1. Create an openshift account and create an application using the d.i.y. cart.
   For now we name it test so that the application should be reachable under http://test-youraccountname.rhcloud.com

2. On the success page you will find a command to clone the created app to your local machine
   Go to a command line where git is available and run the command

3. Go to the directory. There you'll find these directories:

   .git/
   
   .openshift/
   
   diy/
   
   misc/
   You can delete the diy and the misc directory.

4. Making a spring boot application out of that:

   a) copy the <a href="https://github.com/packowitz/openshift-spring-boot-java8/blob/master/pom.xml">pom.xml</a> to the root of your project
      
      It contains only the dependency to spring boot web starter and requires Java8

   b) create a src/main/java/com/example directory and go into that directory
   
   c) add a simple <a href="https://github.com/packowitz/openshift-spring-boot-java8/blob/master/src/main/java/com/example/Application.java">Application.java</a> that contains a main class to start your spring boot application
      This would already be a spring boot application. But a boring one. So we add a little bit of functionality
      
   d) We need a dummy <a href="https://github.com/packowitz/openshift-spring-boot-java8/blob/master/src/main/java/com/example/User.java">User</a> with an id and a name
   
   e) Then we want to post a new User and get that user from a controller. So we add a <a href="https://github.com/packowitz/openshift-spring-boot-java8/blob/master/src/main/java/com/example/UserController.java">UserController</a>. We annotate the class with @RestController and a @RequestMapping("user"). Then add to methods for posting and getting a user.
   
5) Telling opoenshift to how to build start and stop the application
   
   a) create <a href="https://github.com/packowitz/openshift-spring-boot-java8/blob/master/.openshift/action_hooks/build">./openshift/action_hooks/build</a>
      
      on some systems you need to make it executable with: git update-index --chmod=+x .openshift/action_hooks/build
      
      in the build step we already have to care about that we execute the mvn package with java8 because we have the java version named in our <a href="https://github.com/packowitz/openshift-spring-boot-java8/blob/master/pom.xml">pom.xml</a> file. Openshift has already an installed Java8 but in the built-in maven the dependency to Java7 is hard wired. So we need to download our own maven with wget and store it in our $OPENSHIFT_DATA_DIR so that it gets not lost the next time we push something to our repo. Then we nee to create an .m2/repository directory (if it's not already exists) and write a simple settings.xml in the .m2 directory. Then we set the JAVA_HOME to Java8 and execute our freshly downloaded mvn clean package
      
   b) edit the <a href="https://github.com/packowitz/openshift-spring-boot-java8/blob/master/.openshift/action_hooks/start">.openshift/action_hooks/start</a> action hook to start our spring boot application using the installed java8
   
   c) edit the <a href="https://github.com/packowitz/openshift-spring-boot-java8/blob/master/.openshift/action_hooks/stop">.openshift/action_hooks/stop</a> action hook to look for the java process to kill it
   
6) commit and push your changes to openshift. After that you should be able to post a user to http://test-youraccountname.rhcloud.com/user and retrieve it back with http://test-youraccountname.rhcloud.com/user/{id}
