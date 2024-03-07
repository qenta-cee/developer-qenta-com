# Java SDK

The following steps are required to install the Qenta SDK server in a Windows Server 2016 computer.

> **BEFORE YOU START**
>
> Make sure the server has internet access to download the required JRE files and administrative access to your
> Windows Server.

{style="note"}

<procedure title="Procedure" id="manual-integration">
    <step>
        <p>Download JRE 21 and select a procedure type from the completion suggestions:</p>
        <list>
            <li> Open your web browser and visit the Adoptium website to download JRE 21 manually.</li>
            <li> Use the following URL: 
                <a href="https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.1+12/OpenJDK21U-jre_x64_windows_hotspot_21.0.1_12.zip">Adoptium Temurin JRE 21</a>
            </li>
            <li>Save the downloaded file to a location on your server.</li>
        </list>
    </step>
    <step>
        <p>Create Target Directory </p>
        <list>
            <li>Open Command Prompt as an administrator</li>
            <li> <p>Run the following command to create a directory for Java</p>
            <code>mkdir C:\Java\jre-21</code>
            </li>
        </list>
    </step>
    <step>
    <p>Extract JRE Files </p>
        <list>
            <li>Navigate to the directory where you saved the downloaded ZIP file</li>
            <li> <p> Right-click on the ZIP file, choose <shortcut>Extract All </shortcut> and select the target directory as C:\Java </p>
            </li>
        </list>
    </step>
    <step>
    <p>Set <code>JAVA_HOME</code> </p>
        <list>
            <li>Open Command Prompt as an administrator.</li>
            <li> <p> Run the following command to set the <code>JAVA_HOME</code> environment variable</p>
                <code>setx JAVA_HOME "C:\Java\jre-21" /M</code>
            </li>
        </list>
    </step>
    <step>
    <p>Copy JAR Files</p>
        <list>
            <li>Open a new Command Prompt as an administrator.</li>
            <li> <p> Run the following command to copy JAR files to the designated location</p>
            <code-block lang="bash" ignore-vars="true">
                xcopy /Y /S /E C:\Java\jdk-21.0.1+12-jre\* %JAVA_HOME%\
                </code-block> 
            </li>
        </list>
    </step>
    <step>
    <p>Update PATH Variable to persist across restarts</p>
    <code-block lang="bash"  ignore-vars="true">
    setx PATH "%PATH%;%JAVA_HOME%\bin" /M
    </code-block>
    </step>
    <step>
    <p>Verify Installation</p>
    <list>
    <li> <p>Open a new Command Prompt as an administrator and execute the command: </p>
    <code-block lang="bash" ignore-vars="true">
    setx PATH "%PATH%;%JAVA_HOME%\bin" /M
    </code-block>        
<code></code>
    </li>
    </list>
    </step>
    <step>
    <p>Depending on the configuration type, set the environment variables if using <code>ENVIRONMENT VARIABLE</code> or
    <code>JSON_FILE</code></p>
       <list>
       <li> <p>CONFIGURATION_TYPE= <code>ENVIRONMENT_VARIABLE </code></p>
        <code-block>
            setx ENVIRONMENT_CONFIG_URL https://config-url /M
            setx QENTA_ACCESS_TOKEN token /M
            setx QENTA_ENVIRONMENT DEVELOPMENT /M
            setx QENTA_ORGANIZATION_ID 2648 /M
            setx QENTA_PRIVATE_KEY privateKey /M
            setx QENTA_USER_ID 6730 /M
        </code-block>
        <p>Then, restart the command line and execute:</p>
        <code>java -jar qenta-sdk-server-1.0.0.jar</code>
       </li>
       <li> <p>CONFIGURATION_TYPE= <code>JSON_FILE </code></p>
        <code-block>
            java -jar qenta-sdk-server-1.0.0.jar
            --sdk.json-file-path=path\to\your\json\file\configuration.json
            --sdk.configuration-source-type=JSON_FILE
        </code-block>       
        </li>
       </list>
    </step>
</procedure>

> **Optional**
>
> add: --server.port=custom_port_number
>
{style="tip"}


## Qenta Integration Using Windows Batch Scripts

The following steps are required to install the Qenta SDK server in a Windows Server 2016 computer using two batch
files, one that installs JRE 21 and another one that runs the SDK Server.


<procedure title="Installation" id="batch-script-integration">
    <step>
        <p>Download and Install Java</p>
    <list>
    <li>Open the <code>install.bat</code> file and ensure that the DOWNLOAD_URL points to the correct Java version
    </li>
    <li> Run the <code>install.bat</code> file as an administrator by right-clicking and selecting <shortcut>Run as administrator.</shortcut>
    </li>
    <li>The script will download and install Java. Once completed, it will set up the necessary
environment variables.</li>
    </list>
    </step>
    <step><p>Prepare the Server JAR:</p>
        <list>
            <li>Place your Spring server JAR file <code>(e.g., qenta-sdk-server-1.0.0.jar)</code> and the corresponding
            JSON configuration file <code>(e.g., configuration.json) </code>in the same directory as the batch files
            </li>
        </list>
    </step>
    <step>
        <p>Configure Environment Variables</p>
        <list>
            <li>Navigate to the directory containing the batch files and execute the following commands to
set environment variables if you are going to use </li>
        </list>
    </step>
    <step>
        <p>Prepare the Server JAR:</p>
        <list>
            <li>Place your Spring server JAR file <code>(e.g., qenta-sdk-server-1.0.0.jar)</code> and the corresponding
            JSON configuration file <code>(e.g., configuration.json) </code>in the same directory as the batch files
            </li>
        </list>
    </step>
    <step>
        <p>Configure Environment Variables</p>
        <list>
            <li>Open a new command prompt as an administrator</li>
            <li>Navigate to the directory containing the batch files and execute the following commands to set environment variables, when using: <code>CONFIGURATION_TYPE=ENVIRONMENT_VARIABLE</code> 
            <code-block>
                setx ENVIRONMENT_CONFIG_URL https://config-url/dev /M
                setx QENTA_ACCESS_TOKEN token /M
                setx QENTA_ENVIRONMENT DEVELOPMENT /M
                setx QENTA_ORGANIZATION_ID 2648 /M
                setx QENTA_PRIVATE_KEY privateKey /M
                setx QENTA_USER_ID 6730 /M
            </code-block>
            </li>
            <li>Close and reopen the command prompt to apply the changes.</li>
        </list>
    </step>
    <step>
        <p>Run the Server</p>
        <list>
            <li>Double-check that the JAR file <code>(qenta-sdk-server-1.0.0.jar)</code> and the JSON configuration file
<code>(configuration.json)</code> are in the same directory as the batch files.</li>
            <li>Update the execute.bat file depending on your preferred configuration source type 
            <code-block>
                set CONFIGURATION_TYPE=JSON_FILE
                set CONFIGURATION_TYPE=ENVIRONMENT_VARIABLE
            </code-block>
            </li>
            <li>Run the <code>execute.bat</code> file as an administrator by right-clicking and selecting <shortcut>Run as
administrator</shortcut></li>
        </list>
    </step>
</procedure>