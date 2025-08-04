# 🧪 SQLcl MCP with using VS Code and Cline

> 💡 Steps to create SQLcl MCP environment with VS Code and Cline on Macos

## 🌟 Introduction

The Oracle SQLcl Model Context Protocol (MCP) Server transforms how you interact with the Oracle Database by enabling seamless communication with Artificial Intelligence (AI) applications.<br>
It enables you to perform operations, create reports, and run queries on Oracle Database using natural language through AI-powered interactions.<br>
This guide includes information related to how to run initialize and test SQLcl MCP feature with VSCode and Cline on a MacBook. And to test capabilities of SQLcl MCP with some sample prompts.<br>

This guide includes information related how to:

- 📦 Download and install SQLcl for MAC.
- 📦 Download and install VS Code for MAC.
- 📦 Install SQL Developer extension in VS Code.
- 📦 Install and Configure Cline.
- ✅ Connect to database and test the functionality.

> Ideal for DBAs, developers, whom are exploring SQLcl MCP capabilities with VS Code and Cline.

## 🌟 Assumptions

- We need Oracle database connection (In my case Oracle Database 23ai Free image running on Podman on my Mac Notebook, You can use other Oracle Database options as well, like Autonomous Database).<br>
  Instructions related to how to provision Oracle Database 23ai Free container image using Podman on Mac can be found: <br>
  https://github.com/umutnazlica/Podman_Mac_DB23AI_APEX <br>

- I use existing pluggable database and sample HR schema which i created in:
  https://github.com/umutnazlica/MCP_VSCODE_COPILOT

- In my case i use OpenAI API, there should be an OpenAI subscription, enough credit to make API calls (no free option, you need to add payment method and add credit) and need to create an API Key to make API calls from Cline. <br>

  https://openai.com/<br>
  https://platform.openai.com/settings/organization/billing/overview<br>
  https://platform.openai.com/settings/organization/api-keys<br>


## 🌟 Additional Resources

https://docs.oracle.com/en/database/oracle/sql-developer-command-line/25.2/sqcug/using-oracle-sqlcl-mcp-server.html<br>
https://connor-mcdonald.com/2025/07/14/first-dabblings-with-sqlcl-mcp-server/<br>
https://blogs.oracle.com/database/post/introducing-mcp-server-for-oracle-database<br>
https://www.thatjeffsmith.com/archive/2025/07/getting-started-with-our-mcp-server-for-oracle-database/<br>
https://youtu.be/hj6WoZVGUBg <br>
https://blogs.oracle.com/database/post/how-can-developers-and-dbas-benefit-from-mcp-server-for-oracle-database <br>

---

### ⚙️ Setup Instructions for SQLcl

If not already installed and configured: <br>
- Download the latest version of Oracle SQLcl (version 25.2+ required for MCP) and Install. I use brew to install SQLcl. <br>
  If already installed please upgrade SQLcl to latest version, while creating the document latest version is 25.2.2)<br>

  ```bash
  brew install SQLcl
  ```
  SQLcl requires Java 11+. You can install the latest version with:<br>
  ```bash
  brew install --cask temurin
  ```
  ```bash
  export PATH=/opt/homebrew/Caskroom/SQLcl/25.2.2.199.0918/SQLcl/bin:"$PATH"
  ```
  If you prefer manual download of SQLcl downlod link: <br>
  https://www.oracle.com/database/sqldeveloper/technologies/SQLcl/download/ <br>
  please download, extract and copy SQLcl folder to your working directory, and modify export PATH command.<br>

  Check if this version of SQLcl supports MCP? <br>
  ```bash
  ./sql -mcp 
  ```
 > ---------- MCP SERVER STARTUP ----------<br>
 > MCP Server started successfully on Mon Jul 28 13:43:39 GMT+03:00 2025<br>
 > Press Ctrl+C to stop the server <br>
 > ----------------------------------------<br>

---

### ⚙️ Setup Instructions for VS Code and Copilot.

If not already installed and configured:
- Download and install VS Code : https://code.visualstudio.com/download <br>
  Extract the zip file and copy Visual Studio Code to Applications. <br>

- Install latest version of SQL Developer Extension for VSCode:<br>
  https://marketplace.visualstudio.com/items?itemName=Oracle.sql-developer <br>

- Create New and copy or have your existing OpenAI API Key in hand, we will use it in Cline API Provider configuration.<br>
  You can see your API Keys, credits from your OpenAI profile page: <br>
  https://platform.openai.com/settings/profile/user<br>

- Install and configure Cline from VSCode Extensions Marketplace:<br>
  Follow the instructions on documentation: <br>
  https://docs.oracle.com/en/database/oracle/sql-developer-command-line/25.2/sqcug/starting-and-managing-sqlcl-mcp-server.html#GUID-42167832-B364-4A3E-8A17-9FAE1F6CCFD3 <br>
  
  Configuration of API Provider and Model: <br>
  API Provider: OpenAI<br>
  OpenAIAPIKey: Paste Your API Key<br>
  Model: gpt-4.1<br>

  Follow instructions to add SQLcl MCP Server to Cline: <br>
  Modify the path with your SQLcl folder location! In my case cline_msp_settings.json config : <br>
  ```bash
  {
   "mcpServers": {
    "SQLcl": {
            "command": "/opt/homebrew/Caskroom/sqlcl/25.2.2.199.0918/sqlcl/bin/sql",
            "args": ["-mcp"],
            "disabled": false
        }
    }
  }
  ```
  Your Cline MCP Servers should look like: <br>

  ![Image](https://github.com/user-attachments/assets/e15ff979-7d28-4efe-9a6c-681309b47b50)


  I used and test the SQLcl MCP functionality with connections i have to a containerized database running on my Mac Podman environment. Some prompts to test via Cline + SQLcl MCP:  <br>
  ```bash
  list my database connections
  ```
  ![Image](https://github.com/user-attachments/assets/04864ee9-7e5e-4667-93ce-6d0176968fb8)

 ```bash
  connect to database oracledb_mcp_hr
  ```
  > Successfully connected to the Oracle database "oracledb_mcp_hr". The connection is active and ready for use. The database version is 23.0.0.0.0, and the NLS parameters (including character set AL32UTF8) have been provided for accurate data handling.

  ```bash
  are there any sensitive data hosted on HR schema, can you list me the table name, column name and records holds sensitive data
  ```

  >The query identified the following columns in the HR schema that may contain sensitive data:<br>
  >- DBTOOLS$MCP_LOG: ID<br>
  >- DEPARTMENTS: ID<br>
  >- EMPLOYEES: DEPARTMENT_ID, ID, JOB_ID, LOCATION_ID, SALARY<br>
  >- JOBS: ID<br>
  >- JOB_HISTORY: EMPLOYEE_ID, ID, JOB_ID<br>
  >- LOCATIONS: ID<br>
  >Of these, the most likely to contain sensitive data is EMPLOYEES.SALARY. The other columns (ID, DEPARTMENT_ID, etc.) are identifiers or foreign keys, which are not typically considered sensitive unless they can be linked to personal     information.<br>
  >To fulfill the user's request, I will:<br>
  >1. Query the EMPLOYEES table for the SALARY column and show a sample of records.<br>
  >2. For completeness, I will also show a sample of the other columns, but note that they are not typically sensitive.<br>
  >I will start by querying EMPLOYEES for the SALARY column and a few other columns to provide context.<br>

  >Analysis of the HR schema for sensitive data:<br>
  >The following table and column in the HR schema contain sensitive data:<br>
  >- Table: EMPLOYEES<br>
  >- Column: SALARY<br>
    >- Example records:<br>
      >- ID: 1, SALARY: 50000<br>
      >- ID: 2, SALARY: 60000<br>
      >- ID: 3, SALARY: 70000<br>
      >- ID: 4, SALARY: 12000<br>
      >- ID: 5, SALARY: 14000<br>
  >Other columns identified (such as ID, DEPARTMENT_ID, JOB_ID, LOCATION_ID, EMPLOYEE_ID) are identifiers or foreign keys and are not typically considered sensitive unless linked to personal information. No columns matching patterns for email, phone, SSN, address, or other direct personal identifiers were found in the HR schema.<br>
  >Summary: The only sensitive data found is in the EMPLOYEES.SALARY column.<br>

 ```bash
  create me a pie chart which shows employee count by department
  ```
First i got this first response , indicating that the in the query it has created, column department_name does not exist in departments table:<br>
>Error starting at line : 2 in command -
>SELECT /* LLM in use is gpt-4.1 */ d.department_name, COUNT(e.id) AS employee_count FROM hr.departments d LEFT JOIN hr.employees e ON d.id = e.department_id GROUP BY d.department_name ORDER BY employee_count DESC
>Error at Command Line : 2 Column : 167
>Error report -
>SQL Error: ORA-00904: "D"."DEPARTMENT_NAME": invalid identifier
>https://docs.oracle.com/error-help/db/ora-00904/00904. 00000 -  "%s: invalid identifier"
>*Cause:    The identifier or column name entered was invalid.
>*Action:   Ensure the following
>More Details :
>https://docs.oracle.com/error-help/db/ora-00904/

>The error ORA-00904: "D"."DEPARTMENT_NAME": invalid identifier indicates that the column DEPARTMENT_NAME does not exist in the HR.DEPARTMENTS table, or the column name is different (possibly named differently or in a different case).
>To resolve this, I should first check the actual column names in the HR.DEPARTMENTS table. I will query the data dictionary for all columns in HR.DEPARTMENTS to find the correct column name for the department.

Then it finds the correct column name from data dictionary and modified the query <br>
>The HR.DEPARTMENTS table has columns: ID and NAME. The correct column for the department name is NAME, not DEPARTMENT_NAME. I will update the query to use d.NAME instead of d.DEPARTMENT_NAME and rerun the employee count by department query.

Sucessfully executed the query and get the results from database: <br>

>I now have the data needed for the pie chart:
>- HR: 2 employees
>- IT: 2 employees
>- Finance: 1 employee
>I will create an HTML file with embedded Chart.js to visualize this as a pie chart. The file will be named employee_count_by_department.html and will be self-contained for easy viewing in a browser.

After approving for new file generation successfully created the pie chart as a new html file and opened it: <br>


![Image](https://github.com/user-attachments/assets/86ac6f44-65c9-460f-a9cc-04cbaf162c1c)


Some recommendations as suggested in the blogs and official documentation:
  - Think twice and hard before ever letting the tool connect to a production or any critical database environment.
  - Don’t let the tool connect to an account with any kind of privilege that could damage your data/database. Use least privilage principal! This will limit what is accessible to the LLM.
  - Always review requested actions before approving them. Do not use 'Always Allow' when asked for confirmation, verify first.

### 🔧 Configuration

1. **Clone this repository**  
   ```bash
   git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
   cd YOUR_REPO
  ```
