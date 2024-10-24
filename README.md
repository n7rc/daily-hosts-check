**\# Daily Hosts Check Project**

**\## .SYNOPSIS**

Automates the daily management of Windows hosts, including power-on, shutdown, system load monitoring, session detection, event log capture, mandatory updates, hardware inventory collection, and generating reports in HTML, CSV, and SQL queries for database injection. Also supports email notifications to the system administrator.

**\## .DESCRIPTION**

This script automates several key administrative tasks for managing a group of Windows hosts, while also incorporating user logon functionality for secured access to the dashboard:

**\### 1. \*\*Logon Management:\*\***

\- Ensures users are authenticated before accessing the dashboard by verifying credentials via \`logonProcess.php\`.

\- Handles session creation, validation, and automatic redirection on logon success or failure.

\- Displays an error message and redirects to the login page (\`logon.php\`) if credentials are incorrect.

**\### 2. \*\*Host Management:\*\***

\- Powers on hosts from a specified group (default: "all").

\- Monitors CPU, memory, and disk usage for the powered-on hosts, including alerts when usage exceeds 80%.

\- Detects active user sessions on the hosts.

\- Attempts to shut down hosts that have no active sessions.

**\### 3. \*\*Updates and Event Logs:\*\***

\- Automatically installs updates on successfully powered-on hosts:

&nbsp;   - If \`-updateHosts\` is not specified, updates are installed using internal Windows commands (beta).

&nbsp;   - If \`-updateHosts\` is set to \`psupdate\`, updates are installed using the PSWindowsUpdate module.

\- Captures critical events (Level 1) from System and Setup logs, and error events (Level 2) from Application logs from the last 7 days.

**\### 4. \*\*Reports and Notifications:\*\***

\- Uses stored credentials for remote operations, including PowerShell remoting and Windows Update management.

\- Generates comprehensive reports in HTML with print preview and PDF export functionalities.

\- Exports host metrics, hardware inventory, and summary data to CSV and log files for easy data tracking and sharing.

\- Prepares SQL queries for host metrics, hardware details, and summary data, enabling easy database injection for further analysis.

\- Sends email notifications to the system administrator, summarizing the results of the daily operations, including power-on and shutdown status, system resource usage, update results, and hardware inventory.

**\## .EXAMPLES:**

\`\`\`powershell

.\\dailyHostsCheck.ps1 -g "all" -updateHosts "psupdate"

.\\dailyHostsCheck.ps1 -g "administration" -csvFilePathHosts ".\\data\\csv\\hostMetrics.csv"

.\\dailyHostsCheck.ps1 -g "labs" -debug

\## .RECOMMENDED EXAMPLE:

\`\`\`powershell

.\\dailyHostsCheck.ps1 -g all -updateHosts psupdate -debug

\## .PARAMETER -g

Specifies the group of hosts to manage. The default value is "all."

\## .PARAMETER -updateHosts \["psupdate", ""\]

If specified with the value psupdate or not specified -updateHosts, the script will install updates using the PSWindowsUpdate module.

If specified -updateHosts "", updates will be installed using internal Windows commands.

\## .PARAMETER -debug

Enables debug mode, displaying detailed logs and messages for troubleshooting.

**\## .NOTES

\- Requires administrative privileges to perform certain remote operations.

\- PowerShell remoting must be enabled on target hosts for remote execution.

\- Uses stored credentials loaded from an XML file for remote authentication and execution.

\- Uses stored credentials loaded from an XML file for dashboard access authentication.

\- The PSWindowsUpdate module should be installed on target hosts if using the -updateHosts psupdate option. The script installs it by default if -updateHosts psupdate is selected.

\- Monitors system resource usage and flags hosts exceeding 70% CPU, RAM, or Disk usage.

\- Captures critical (Level 1) and error (Level 2) events from the System, Setup, and Application logs.

\- Collects hardware inventory, including details such as CPU, Network Adapter, Motherboard, BIOS, OS, and GPU information.

\- Generates reports in HTML with print preview and PDF export functionalities, and exports host data to CSV, log, and SQL formats.

\- Sends an email notification to the system administrator with a summary of the daily checks, logs, update results, and hardware inventory.

\- The scripts \`activeSessions.ps1\`, \`powerOnHosts.ps1\`, and \`shutdownHosts.ps1\` are auxiliary scripts for the main module, but they can be used independently by passing the        corresponding parameters in each script. These scripts are self-documented.

\- Other auxiliary scripts accompany the application, which, while part of its workflow, can be used separately. They are located in the \`bin\` folder and include: \`captureEvents.ps1\`, \`checkHostLoad.ps1\`, and \`checkHostState.ps1\`. These scripts are self-documented.

**\## .FILE STRUCTURE

│   .htaccess

│   activeSessions.ps1

│   dailyHostsCheck.ps1

│   powerOnHosts.ps1

│   README.md

│   shutdownHosts.ps1

├───bin

│       captureEvents.ps1

│       checkHostLoad.ps1

│       checkHostState.ps1

├───cfg

│   │   hostGroupsFull.json

│   │   hostGroupsMACFull.json

│   ├───database

│   │       createDataBase.sql

│   └───gpo

│           Configure WinRM and PSRemoting GPO.ps1

├───cred

│       credAdmin.xml

│       credPSRemotingLocal.xml

├───dashboard

│   │   dashboard.php

│   │   hardwareInventory.php

│   │   logon.php

│   │   logonProcess.php

│   ├───css

│   │       dailyhostscheck.css

│   └───js

│           dailyhostscheck.js

├───data

│   ├───csv

│   │   └───sessions

│   └───log

├───lib

│       checkHostLoadMonitorTask.ps1

│       createHostGroupsFull.ps1

│       createHostGroupsMACFull.ps1

│       dailyHostCheckTask.ps1

│       dataOutput.ps1

│       hostGroupsMACTemplate.json

│       hostGroupsTemplate.json

│       sendMail.ps1

├───man

└───tmp

**\## .ORGANIZATION

Developed for: Faculty of Documentation and Communication Sciences at the University of Extremadura.

**\## .AUTHOR

Alberto Ledo \[Faculty of Documentation and Communication Sciences\] - with assistance from OpenAI  

IT Department: University of Extremadura - IT Services for Facilities  

Contact: <albertoledo@unex.es>

**\## .VERSION

2.2.0

**\## .HISTORY

\- 1.0 - Initial version by Alberto Ledo.

\- 1.1 - Added email reporting for power-on, shutdown, and session detection.

\- 1.2 - Introduced the \`-updateHosts\` parameter for optional host update functionality.

\- 1.3 - Integrated the \`-g\` parameter for specifying host groups, with "all" as default.

\- 1.4 - Added the use of stored credentials for remote operations and enhanced logging features.

\- 1.5 - Included host load monitoring (CPU, RAM, Disk) and resource usage alerts in email reports.

\- 1.6 - Integrated the PSWindowsUpdate module for host updates based on the \`-updateHosts\` parameter.

\- 1.7 - Added the retrieval of active session details on hosts.

\- 1.8 - Added critical and error log capture from System, Setup, and Application logs.

\- 1.9 - Introduced HTML report generation with print preview and PDF export functionality.

\- 2.0 - Added CSV and log export for host and summary data, and generated SQL queries for database injection.

\- 2.1 - Added hardware inventory functionality, including CPU, Network Adapter, Motherboard, BIOS, OS, and GPU collection.

\- 2.2 - Added user logon functionality for secured access control to the dashboard.

**\## .USAGE

This script is strictly for internal use within Universidad de Extremadura.  

The script is designed to operate within the IT infrastructure and environment of Universidad de Extremadura and may not function as expected in other environments.

**\## .DATE

October 23, 2024 16:30

**\## .DISCLAIMER

This script is provided "as-is" and is intended for internal use at Universidad de Extremadura.  

No warranties, express or implied, are provided. Any modifications or adaptations are not covered under this disclaimer.

**\## .LINK

\- \[Daily Hosts Check on GitHub\](<https://github.com/n7rc/daily-hosts-check>)

\- \[Daily Hosts Check Documentation\](<https://n7rc.gitbook.io/>)