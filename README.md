# Application-Health-Checker-
Please write a script that can check the uptime of an application and determine if it is functioning correctly or not. The script must accurately assess the application's status by checking HTTP status codes. It should be able to detect if the application is 'up', meaning it is functioning correctly, or 'down', indicating that it is unavailable.
-----------------------------------

import requests
import pandas as pd

# Read the Excel file into a DataFrame
file_path = r'C:\Users\brije\OneDrive\Desktop\Clarity\Python\output\report.xlsx'
df = pd.read_excel(file_path)

# Iterate through the DataFrame
for index, row in df.iterrows():
    try:
        # Send a GET request to the website
        response = requests.get(row["URL"], timeout=10)
        
        # Check if the status code is 200 (OK)
        if response.status_code == 200:
            df.at[index, "status_code"] = f'Site is Up ({response.status_code})'
        else:
            df.at[index, "status_code"] = f'Site is Down ({response.status_code})'
    
    except requests.exceptions.RequestException:
        # If an exception occurs (timeout, network issue), mark the site as Down
        df.at[index, "status_code"] = "Site is not available"

# Save the updated DataFrame back to the Excel file
df.to_excel(file_path, index=False)

# Print the updated DataFrame
print(df)



