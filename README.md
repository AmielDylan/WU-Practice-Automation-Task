# ParaBank Automation – REFramework Project  

## 📌 Description  
This UiPath project automates user scenarios on the **ParaBank** demo banking site using the **Robotic Enterprise Framework (REFramework)**.  
The solution is divided into two parts:  
- **Dispatcher**: loads user data and pushes it into an Orchestrator Queue.  
- **Performer**: processes each user transaction from the Queue.  

---

## 🏗 Architecture  
- **Dispatcher**  
  - Reads the provided CSV file.  
  - Validates input data.  
  - Adds one `QueueItem` per user to the Orchestrator Queue defined in `Config.xlsx`.  

- **Performer (REFramework)**  
  - Retrieves a transaction from the Queue.  
  - Executes the following workflow steps:  
    1. **Register** – Create a ParaBank account.  
    2. **Open Account** – Open a new account with the initial deposit.  
    3. **Request Loan** – Request a loan of **10,000 USD** with a down payment equal to **20% of the initial deposit**.  
    4. **Report** – Generate a consolidated report with USD→EUR conversion.  
    5. **Logout & Close** – End session cleanly.  

---

## ⚙️ Configuration  
All configurable values are stored in **`Config.xlsx`** (located in the `Data/` folder).  

### Key Sections  
- **Settings**  
  - `OrchestratorQueueName`: main queue for ParaBank users.  
  - `ReportingQueue`: queue for reporting data.  
  - `StorageBucketName`: Orchestrator storage bucket for report files.  
  - `MaskReportSensitiveData`: `True/False` → defines whether sensitive data (password, CVV, credit card) should be masked in the report.  

- **Constants**  
  - `MaxRetryNumber`, `MaxConsecutiveSystemExceptions`, retry parameters for GetTransactionItem/SetTransactionStatus.  
  - Standard log messages.  
  - `ExScreenshotsFolderPath`: folder for saving screenshots in case of exceptions.
  
---

## 🔒 Security  
- Passwords, CVVs, and credit card numbers are **masked by default** in the generated report.  
- Credentials are retrieved from **Orchestrator Assets** or **Windows Credential Manager**.  
- No Personally Identifiable Information is logged in clear text.  

---

## 🚀 Execution Steps  
1. Publish the project to Orchestrator.  
2. Create necessary **Assets** in Orchestrator:  
3. Create required **Queues**:  
   - `ParaBank Users` (main user transactions).  
   - `Reporting Queue` (report data).   
4. Run the process to populate the Queue and process transactions.
5. **Dispatcher** is integrated to the **REF**

---

## 📊 Reporting  
The automation generates an Excel file (default: `Data/Output/Automation Summary.xlsx`) containing:  
- Customer details (name, masked email, DOB).  
- Created account details (account number, balance).   
- Conversion into EUR.  
- Transaction status. 

