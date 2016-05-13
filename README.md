# openshift-spring-boot-java8
This is an example how to setup an openshift application using Java8 and Spring Boot

1. Create an openshift account and create an application using the d.i.y. cart.
   For now we name it test so that the application should be reachable under http://test-youraccountname.rhcloud.com

2. On the success page you will find a command to clone the created app to your local machine
   Go to a command line where git is available and run the command

3. Go to the directory. There you'll find these directories:
   |-.git
   |-.openshift
   |-diy
   |-misc
   You can delete the diy and the misc directory.

4. Making a spring boot application out of that:
   a) copy the pom.xml to the root of your project
      It contains only the dependency to spring boot web starter and requires Java8

   b) create a src/main/java/com/example directory and go into that directory
   
   c) add a simple Application.java that contains a main class to start your spring boot application
      This would already be a spring boot application. But a boring one. So we add a little bit of functionality
      
   d) We a dummy User with an id and a name
   
   e) Then we want to post a new User and get that user from a controller. So we add a UserController. We annotate the class with @RestController and a @RequestMapping("user"). Then add to methods for posting and getting a user.
   
5) Telling opoenshift to how to build start and stop the application
   a) create ./openshift/action_hooks/build
      on some systems you need to make it executable with: git update-index --chmod=+x .openshift/action_hooks/build
