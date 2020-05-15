# Step 2: Create the source code<a name="getting-started-cli-create-source-code"></a>

\(Previous step: [Step 1: Create two S3 buckets](getting-started-cli-input-bucket.md)\)

In this step, you create the source code that you want CodeBuild to build to the output bucket\. This source code consists of two Java class files and an Apache Maven Project Object Model \(POM\) file\.

1. In an empty directory on your local computer or instance, create this directory structure\.

   ```
   (root directory name)
       `-- src
            |-- main
            |     `-- java
            `-- test
                  `-- java
   ```

1. Using a text editor of your choice, create this file, name it `MessageUtil.java`, and then save it in the `src/main/java` directory\.

   ```
   public class MessageUtil {
     private String message;
   
     public MessageUtil(String message) {
       this.message = message;
     }
   
     public String printMessage() {
       System.out.println(message);
       return message;
     }
   
     public String salutationMessage() {
       message = "Hi!" + message;
       System.out.println(message);
       return message;
     }
   }
   ```

   This class file creates as output the string of characters passed into it\. The `MessageUtil` constructor sets the string of characters\. The `printMessage` method creates the output\. The `salutationMessage` method outputs `Hi!` followed by the string of characters\.

1. Create this file, name it `TestMessageUtil.java`, and then save it in the `/src/test/java` directory\.

   ```
   import org.junit.Test;
   import org.junit.Ignore;
   import static org.junit.Assert.assertEquals;
   
   public class TestMessageUtil {
   
     String message = "Robert";    
     MessageUtil messageUtil = new MessageUtil(message);
      
     @Test
     public void testPrintMessage() {      
       System.out.println("Inside testPrintMessage()");     
       assertEquals(message,messageUtil.printMessage());
     }
   
     @Test
     public void testSalutationMessage() {
       System.out.println("Inside testSalutationMessage()");
       message = "Hi!" + "Robert";
       assertEquals(message,messageUtil.salutationMessage());
     }
   }
   ```

   This class file sets the `message` variable in the `MessageUtil` class to `Robert`\. It then tests to see if the `message` variable was successfully set by checking whether the strings `Robert` and `Hi!Robert` appear in the output\.

1. Create this file, name it `pom.xml`, and then save it in the root \(top level\) directory\.

   ```
   <project xmlns="http://maven.apache.org/POM/4.0.0" 
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
     <modelVersion>4.0.0</modelVersion>
     <groupId>org.example</groupId>
     <artifactId>messageUtil</artifactId>
     <version>1.0</version>
     <packaging>jar</packaging>
     <name>Message Utility Java Sample App</name>
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>	
     </dependencies>
     <build>
       <plugins>
         <plugin>
           <groupId>org.apache.maven.plugins</groupId>
           <artifactId>maven-compiler-plugin</artifactId>
           <version>3.8.0</version>
         </plugin>
       </plugins>
     </build>
   </project>
   ```

   Apache Maven uses the instructions in this file to convert the `MessageUtil.java` and `TestMessageUtil.java` files into a file named `messageUtil-1.0.jar` and then run the specified tests\. 

At this point, your directory structure should look like this\.

```
(root directory name)
    |-- pom.xml
    `-- src
         |-- main
         |     `-- java
         |           `-- MessageUtil.java
         `-- test
               `-- java
                     `-- TestMessageUtil.java
```

## Next step<a name="getting-started-cli-create-source-code-next"></a>

[Step 3: Create the buildspec file](getting-started-cli-create-build-spec.md)