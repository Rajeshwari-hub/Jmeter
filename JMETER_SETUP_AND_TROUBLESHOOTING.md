# Apache JMeter Setup and Troubleshooting

## Prerequisites

- Windows, macOS, or Linux
- Java 8 or later
- Java 11 or 17 is recommended for JMeter 5.6.3

Check Java from Command Prompt:

```cmd
java -version
```

If Java is not installed, download a JDK from [Eclipse Adoptium](https://adoptium.net/).

## Install JMeter

1. Download the binary `.zip` archive from the [Apache JMeter download page](https://jmeter.apache.org/download_jmeter.cgi).
2. Extract it, for example, to:

   ```text
   C:\Users\prade\OneDrive\Documents\Automation\jmeter\apache-jmeter-5.6.3
   ```

3. Open Command Prompt and move to the `bin` directory:

   ```cmd
   cd C:\Users\prade\OneDrive\Documents\Automation\jmeter\apache-jmeter-5.6.3\bin
   ```

4. Start the JMeter graphical interface:

   ```cmd
   jmeter.bat
   ```

## Create a First Test Plan

1. Open JMeter and select **File > New**.
2. Right-click **Test Plan**, then select **Add > Threads (Users) > Thread Group**.
3. Set the number of threads, ramp-up period, and loop count.
4. Right-click the Thread Group, then select **Add > Sampler > HTTP Request**.
5. Enter the target server, port, protocol, path, and HTTP method.
6. Add **View Results Tree** for local debugging.
7. Select **Run > Start**.
8. Save the test plan as a `.jmx` file.

## Run from the Command Line

Use non-GUI mode for load tests:

```cmd
.\bin\jmeter.bat -n -t .\test-plan.jmx -l .\results\results.jtl -e -o .\results\report
```

The options mean:

- `-n`: run without the GUI
- `-t`: specify the JMX test plan
- `-l`: write sample results to a JTL file
- `-e -o`: generate an HTML report

Open this file after the test completes:

```text
results\report\index.html
```

## Troubleshooting `findstr` and Java Errors

If `jmeter.bat` displays:

```text
'findstr' is not recognized as an internal or external command,
operable program or batch file.
Not able to find Java executable or version. Please check your Java installation.
errorlevel=2
```

The Java error can be misleading. JMeter uses the Windows `findstr` command while checking the Java version. If `findstr` cannot be found, JMeter may report that Java is missing even when Java is installed correctly.

### 1. Check Java and `findstr`

Open a **new Command Prompt** and run:

```cmd
where findstr
where java
java -version
```

`findstr` should resolve to:

```text
C:\Windows\System32\findstr.exe
```

If `java -version` succeeds but `where findstr` fails, Java is installed and the Windows `PATH` is missing the System32 folder.

### 2. Temporary fix

Run this in the current Command Prompt:

```cmd
set "PATH=%SystemRoot%\System32;%PATH%"
findstr /?
```

If help text appears, start JMeter again:

```cmd
cd C:\Users\prade\OneDrive\Documents\Automation\jmeter\apache-jmeter-5.6.3\bin
jmeter.bat
```

This fix applies only to the current Command Prompt window.

### 3. Permanent PATH fix

1. Search Windows for **Edit the system environment variables**.
2. Open **Environment Variables**.
3. Under **System variables**, select `Path` and click **Edit**.
4. Add these entries if they are missing:

   ```text
   C:\Windows\System32
   C:\Windows
   C:\Windows\System32\Wbem
   ```

5. Make sure the JDK `bin` directory is also present, for example:

   ```text
   C:\Program Files\Java\jdk-17\bin
   ```

6. Create or edit `JAVA_HOME` and set it to the JDK folder, not the `bin` folder:

   ```text
   C:\Program Files\Java\jdk-17
   ```

7. Click **OK** on all dialogs.
8. Close all existing Command Prompt windows and open a new one.

Verify the permanent fix:

```cmd
echo %JAVA_HOME%
where findstr
where java
java -version
```

### 4. Java 25 note

If your output shows Java 25, Java is detected, but Java 25 is newer than the version available when JMeter 5.6.3 was released. If you see compatibility errors after fixing `PATH`, install Java 17 and point `JAVA_HOME` to that JDK for JMeter:

```cmd
set "JAVA_HOME=C:\Program Files\Java\jdk-17"
set "PATH=%JAVA_HOME%\bin;%SystemRoot%\System32;%PATH%"
jmeter.bat
```

## Recommended Practices

- Use the GUI to create and debug test plans.
- Run real load tests in non-GUI mode.
- Do not use **View Results Tree** during a real load test because it consumes significant memory.
- Keep test data and credentials in variables or separate configuration files.
- Start with a small number of users and increase load gradually.
- Review response time, throughput, error rate, and server resource usage together.
