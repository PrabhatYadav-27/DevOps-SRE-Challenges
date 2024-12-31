# Documentation: System Health Monitor Script

This document outlines the challenges faced, solutions implemented, and key concepts learned while developing the **System Health Monitor Script**, which monitors system resources and sends the report via email using `sendmail`.

---

## Overview

The **System Health Monitor Script** is a Bash-based tool for monitoring various system metrics such as disk usage, memory usage, CPU usage, and running services. It generates a health report and can send this report via email using `sendmail`. The script provides an interactive menu to allow users to check different system statistics and email the generated report.

---

## Features

- **Disk Usage**: Checks the available disk space and usage.
- **Memory Usage**: Displays detailed memory usage information.
- **CPU Usage**: Provides CPU usage statistics.
- **Running Services**: Lists currently active services.
- **Email Reporting**: Sends the system health report via email using `sendmail`.
- **Debugging**: Optionally enables debugging mode to log the execution flow for easier troubleshooting.

---

## Challenges Faced

### 1. **Missing `mailx` Package**
   - **Issue**: The default repositories did not include `mailx` on the system, and since the system was not registered, access to additional repositories like EPEL was restricted.
   - **Solution**: Since `mailx` wasn't available, the script was adjusted to use `sendmail`, which is a more common and straightforward mail service available on most Linux distributions.

### 2. **System Registration**
   - **Issue**: The system was not registered with Red Hat Subscription Manager, preventing access to additional repositories.
   - **Solution**: Instead of trying to register the system, the focus was placed on configuring `sendmail` and ensuring it was functional for emailing reports.

### 3. **Lack of Email Configuration**
   - **Issue**: The `sendmail` service wasn't configured by default.
   - **Solution**: Configured `sendmail` by installing it and enabling the service, ensuring the script could send emails without needing external email utilities like `mailx`.

### 4. **Debugging and Script Issues**
   - **Issue**: The script had no debugging output, making it hard to troubleshoot issues.
   - **Solution**: Enabled Bash debugging (`set -x`) to allow the script to output each command it executes, helping identify any errors or unexpected behavior during execution.

---

## Solutions Implemented

### 1. **Configuring `sendmail` for Email Reports**

   - Installed `sendmail` and configured it to send emails:
     ```bash
     sudo yum install -y sendmail sendmail-cf
     sudo systemctl start sendmail
     sudo systemctl enable sendmail
     ```

   - **Email Sending Functionality**: In the script, the `send_email_report()` function was written to generate an email with the system health report and send it via `sendmail`:
     ```bash
     send_email_report() {
         echo "Sending comprehensive report via email..."
         {
             echo "Subject: System Health Report"
             echo "System Health Report:"
             cat $REPORT_FILE
         } | sendmail -i -v -Am $EMAIL

         # Check if the email was sent successfully
         if [ $? -ne 0 ]; then
             echo "Error sending email report!" >> $REPORT_FILE
         else
             echo "Email sent successfully!" >> $REPORT_FILE
         fi
     }
     ```

### 2. **Ensuring System Health Checks Work Properly**

   - The script includes several health checks: disk usage (`df -h`), memory usage (`free -h`), CPU usage (`top -b -n 1 | head -10`), and active services (`systemctl list-units --type=service --state=running`).
   - The report file `/tmp/system_health_status_report.txt` is updated with the output from each check.

### 3. **Debugging Mode**

   - Debugging was enabled with the following code:
     ```bash
     DEBUG=true
     if [ "$DEBUG" = true ]; then
         set -x
     fi
     ```

   - This allows the script to output each command as it's executed, aiding in debugging.

---

## Key Concepts Learned

### 1. **System Administration Skills**
   - Learned how to configure and use `sendmail` for sending emails directly from a Linux system.
   - Gained insights into managing system services like `sendmail` and using system commands (`df`, `free`, `top`, `systemctl`) to monitor system health.

### 2. **Bash Scripting**
   - Improved Bash scripting skills, especially with functions, loops, conditional statements, and file handling.
   - Learned how to use output redirection and append (`>>`) to log data into a file.
   - Gained experience in adding debugging functionality to scripts to track down issues during execution.

### 3. **Email Configuration**
   - Gained knowledge of setting up and configuring `sendmail`, which is useful for sending automated emails directly from a script.
   - Learned how to use `sendmail` in combination with a shell script to send system-generated reports.

### 4. **Error Handling in Bash**
   - Implemented error handling to ensure that email reports are only sent if the system health report was generated successfully.
   - Used the `$?` variable to check the success or failure of commands and perform appropriate actions based on that.

---

## Make the Script Executable:

`chmod +x solution.sh`

## Run the Script:

`./solution.sh`



## Usage
- The script provides an interactive menu for the user to select the system health check to perform:


Copy code
=================================
System Health Check Menu
1. Check Disk Usage
2. Monitor Running Services
3. Assess Memory Usage
4. Evaluate CPU Usage
5. Send Comprehensive Report via Email
6. Exit
=================================
Select an option by entering a number (1-6) and press Enter.

## Email Report
- The email report is sent when the user selects option 5 ("Send Comprehensive Report via Email"). Ensure the sendmail service is running before attempting to send the report.