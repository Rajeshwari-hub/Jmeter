# Apache JMeter Setup

## Prerequisites

- Windows, macOS, or Linux
- Java 8 or later (Java 17 or later is recommended)
- An internet connection to download JMeter and test dependencies

Check that Java is installed:

```powershell
java -version
```

If Java is not installed, install a JDK from [Adoptium](https://adoptium.net/) and restart the terminal.

## Install JMeter

1. Download the latest binary `.zip` archive from the [Apache JMeter download page](https://jmeter.apache.org/download_jmeter.cgi).
2. Extract the archive to a location such as `C:\Tools\apache-jmeter`.
3. Open a terminal in the extracted JMeter directory.
4. Start the graphical interface:

	```powershell
	.\bin\jmeter.bat
	```

For macOS or Linux, run:

```bash
./bin/jmeter
```

## Create a First Test Plan

1. Open JMeter and select **File > New**.
2. Right-click **Test Plan**, then select **Add > Threads (Users) > Thread Group**.
3. Set the number of threads, ramp-up period, and loop count.
4. Right-click the Thread Group, then select **Add > Sampler > HTTP Request**.
5. Enter the target server, port, protocol, path, and HTTP method.
6. Add a listener such as **View Results Tree** for local debugging.
7. Select **Run > Start** to execute the test.
8. Save the test plan as a `.jmx` file.

## Run from the Command Line

Use non-GUI mode for load tests:

```powershell
.\bin\jmeter.bat -n -t .\test-plan.jmx -l .\results\results.jtl -e -o .\results\report
```

The options mean:

- `-n`: run without the GUI
- `-t`: specify the JMX test plan
- `-l`: write sample results to a JTL file
- `-e -o`: generate an HTML report

Open `results\report\index.html` after the run completes.

## Recommended Practices

- Use the GUI to create and debug test plans, then run load tests in non-GUI mode.
- Do not use **View Results Tree** during a real load test because it consumes significant memory.
- Keep test data and credentials in variables or separate configuration files.
- Start with a small number of users and increase load gradually.
- Review response time, throughput, error rate, and server resource usage together.
