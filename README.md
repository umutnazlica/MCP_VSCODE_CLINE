# 🧪 SQLcl MCP with using VS Code and CoPilot

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
https://github.com/umutnazlica/Podman_Mac_DB23AI_APEX

- In my case i use OpenAI API, We need OpenAI subscription, enough credit to make API calls (no free option, you need to add payment method and add credit) and need to create an API Key to make API calls from Cline. 

https://openai.com/
https://platform.openai.com/settings/organization/billing/overview
https://platform.openai.com/settings/organization/api-keys


## 🌟 Additional Resources

https://docs.oracle.com/en/database/oracle/sql-developer-command-line/25.2/sqcug/using-oracle-sqlcl-mcp-server.html<br>
https://connor-mcdonald.com/2025/07/14/first-dabblings-with-sqlcl-mcp-server/<br>
https://blogs.oracle.com/database/post/introducing-mcp-server-for-oracle-database<br>
https://www.thatjeffsmith.com/archive/2025/07/getting-started-with-our-mcp-server-for-oracle-database/<br>
https://youtu.be/hj6WoZVGUBg <br>
https://blogs.oracle.com/database/post/how-can-developers-and-dbas-benefit-from-mcp-server-for-oracle-database <br>

---

### ⚙️ Setup Instructions for SQLcl

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

 Follow instructions to add SQLcl MCP Server to Cline.: <br>
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

I used and test the SQLcl MCP functionality with connecting to a containerized Oracle23ai database running on Podman on my Mac. <br>

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


