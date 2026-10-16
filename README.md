# chicago_food_safety
This project analyzes real-world public health inspection records from the City of Chicago Data Portal to track how quickly food vendors resolve compliance failures.


chicago_zip	| resolved_violations |	median_days_to_compliance
46410     	| 11	                | 717.7
60007	      | 12	                | 486.1
60153	      | 11	                | 162
60193	      | 9	                  | 1109.2
60201     	| 42	                | 1363.6


## How to Run This Locally

Follow these steps to host and run this data analysis project on your own machine using **PostgreSQL** and **pgAdmin 4**.

### Prerequisites
*   **PostgreSQL** (Version 15 or newer recommended)
*   **pgAdmin 4** (Desktop GUI client)

---

### Step 1: Download the Raw Data
1. Navigate to the [City of Chicago Data Portal](https://cityofchicago.org).
2. Click the **Export** button in the top right corner and select **CSV**.
3. Save the file to a known directory on your local PC (e.g., `C:\chicago_data\Food_Inspections.csv`).

### Step 2: Establish the Database Schema
1. Open **pgAdmin 4** and connect to your local PostgreSQL server instance.
2. Under **Databases**, select your target database (or use the default `postgres` database).
3. Right-click the database and select **Query Tool**.
4. Open and execute the script located in this repository at `/scripts/chicago_foodinspection_raw`. This builds the blank baseline data architecture.

### Step 3: Populate the Target Table
1. In the pgAdmin sidebar, expand **Schemas** ➔ **public** ➔ **Tables**.
2. Right-click the newly created `chicago_food_inspections` table and select **Import/Export Data...**
3. Configure the following parameter tabs:
   * **General Tab:** Set the toggle switch to **Import**. Select your local path to the downloaded CSV file. Set the format dropdown to **csv**.
   * **Options Tab:** Set **Header** to **Yes**. Ensure the delimiter is set to a comma (`,`) and the quote character is set to a double quote (`"`).
4. Click **OK** to run the utility pipeline. 
*(Note: If the background process fails with a utility path error, navigate to **File ➔ Preferences ➔ Paths ➔ Binary paths** in pgAdmin and point your PostgreSQL Binary Path to your installation bin folder, e.g., `C:\Program Files\PostgreSQL\[YourVersion]\bin`)*

### Step 4: Build the View Layer and Run Analytics
1. Open a new **Query Tool** window in pgAdmin.
2. Open and execute the script at `/scripts/createview_foodinspection_clean` to instantiate the standardized abstraction view layer (`v_clean_food_inspections`).
3. Open and run `/scripts/avg_days_resolution_per_zip` to execute the regional zip code turnaround metrics query and generate the final reporting data grid.
