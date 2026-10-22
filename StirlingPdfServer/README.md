# Self Hosted stirling pdf setup

## Windows
We are going to use java jar to run the stirling pdf server. because i tried other methods like docker but it was consuming a lot of resources. So i decided to use the java jar approach. but the cache is it dont survive restarts or exit terminal. So we will use winsw to run the jar as a service.
### Step 1: Downlods
1. Download the latest release of [winsw](https://github.com/winsw/winsw/releases) download the latest release.

2. Download the latest release of .jar file [stirling pdf server](https://docs.stirlingpdf.com/Server/Install/JAR/)

3. Install [java 25](https://www.oracle.com/in/java/technologies/downloads/#jdk25-windows) and make sure to add it to your system path. You can check if java is installed by running `java -version` in the command prompt.
### Step 2: Setup
1. create a new folder in your any drive like `Apps\myserver\` and copy the jar file and winsw.exe into that folder.

2. find javaw.exe in your java installation folder. It should be in `C:\Program Files\Java\jdk-25\bin\javaw.exe` or `C:\Program Files\Android\Android Studio\jbr\bin\javaw.exe` if you have android studio installed. Copy the path of javaw.exe. Replace the path in the xml file below with your javaw.exe path. imusing javaw.exe because it will not open a new terminal window when the service is running.

3. Rename the winsw.exe to `stirling-service.exe` and create a new file called `stirling-service.xml` in the same folder. The content of the xml file should be like this:

```xml
<service>
  <id>stirling-pdf</id>
  <name>Stirling-PDF Server</name>
  <description>Stirling-PDF with login running as a native Windows Service.</description>
  <executable>C:\Program Files\Android\Android Studio\jbr\bin\javaw.exe</executable>
  <arguments>-jar "C:\Apps\myserver\Stirling-PDF-with-login.jar"</arguments>
  <workingdirectory>C:\Apps\myserver</workingdirectory>
  
  <!-- Log rotation configuration -->
  <log mode="roll-by-size">
    <sizeThreshold>10240</sizeThreshold>
    <keepFiles>8</keepFiles>
  </log>

  <!-- Automatically restart if it crashes or fails -->
  <onfailure action="restart" delay="10sec"/>
  
  <!-- Start automatically when Windows boots up, even before login -->
  <startupmode>Automatic</startupmode>
</service>
```
### Step 3: Install and Start the Service
1. Open command prompt as administrator and navigate to the folder where you have the `stirling-service.exe` and `stirling-service.xml` files.
2. Run the following command to install the service:
   ```cmd
   stirling-service.exe install
   ```
3. Run the following command to start the service:
   ```cmd
   stirling-service.exe start
   ```