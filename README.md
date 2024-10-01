# Python_Automation_Project
Applying Automation for Data Extraction
- Transitioning from AI bots to Python automation resulted in eliminating all associated costs with data extraction, leading to significant budget savings for the organization.
- The implementation of validation checks in the automation scripts maintains the overall data quality and reliability.

## Web Driver and Selenium Packages
Ensure that the ChromeDriver version matches the version of Chrome, Firefox, or any other browser you're using; otherwise, the driver will not function properly. Additionally, you'll need the Selenium package, which can be downloaded using the link below.

https://www.selenium.dev/

I use Jupyter Notebooks with Anaconda Navigator, which conveniently includes the Selenium package. You can easily find and install it from the Anaconda library. If you're interested in using the same setup, you can download Anaconda Navigator here:

https://www.anaconda.com/

## Web Objective
Ensure you thoroughly inspect the website you plan to extract data from, as each site has unique class names, buttons, spans, IDs, and other elements. If you switch to a different website, these references will vary, requiring you to re-inspect and adjust your extraction script accordingly. Additionally, when automating clicks using JavaScript, modifications may be necessary. Therefore, it's crucial to be 100% certain of the website setup before executing any automation.

## Automation Project Extract
While I am unable to share the complete automation script due to restrictions, I will provide the initial segment and core components of the process. I'll aim to make the explanation as interactive and engaging as possible.

# Automation Code (Python & Java Scripts)
## Importing Libraries
```r
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import NoSuchElementException, ElementClickInterceptedException
from selenium.webdriver.common.action_chains import ActionChains
import time
import csv
options = Options()

# Set a custom user-agent to mimic a real browser
options.add_argument("user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36")
```
## Web Driver
```r
# In Here '###' goes the location path of your driver, DONT FORGET the driver has to be the same version of your browser.
driver = webdriver.Chrome("###")

# In the '##Sign_In##' field, enter the URL of the website where you want to perform data extraction. If the site requires login credentials, navigate to the login page and input your username and password. For security purposes, I recommend using a test account to safeguard your personal information. If does not require Sign In you can go next to 'Extract Function'.

driver.get("##Sign_In##")
```
## Insert Account
```r
# Insert email, In this case the XPATH name is 'session_key' and 'session_password'.In your case you have to inspect the Xpath required for your input.
# I again recommend using a test account to safeguard your personal information
time.sleep(3)
email_input = driver.find_element_by_xpath("//input[@name='session_key']")
email_input.send_keys("YOUR_EMAIL")
time.sleep(3)
email_password = driver.find_element_by_xpath("//input[@name='session_password']")
email_password.send_keys("YOUR_PASWORD")
time.sleep(3)

# Click 'Next' button after inserting email. In this Case I have to click a button to sign in.
signin_button = driver.find_element_by_xpath("//button[@type='submit']")
signin_button.click()
time.sleep(3)
```
## Extract Function
```r

# Define the function to extract information
def extract_information(n):
    result_list = []  # This will store the results of each page
    count = 0

    # Iterate over pages
    for i in range(1, n + 1):
        # Add a 5-second wait before opening the next page
        time.sleep(5)

        # Open the URL for the specific page
        driver.get(f"THE_SPECIFIC_PAGE_YOU_WANT_TO_EXTRACT_INFO")

        # Find all list containers on the page
        all_class = driver.find_elements(By.CLASS_NAME, "reusable-search__result-container")

        # Extract information for each list
        for j in all_class:
            # Split the text by newlines
            text_split = j.text.split("\n")

            # Create a list each row of data extracted, starting with the count
            current_row = [str(count)]

            # Add the split text to the current row list
            current_row.extend(text_split)

            try:
                # Find the link inside this container using XPath, In this case I want to extract the URL that the ROW contains.
                link_element = j.find_element(By.XPATH, './/a[contains(@class, "app-aware-link")]')

                # Get the href attribute of the link and append it to the current list
                href = link_element.get_attribute("href")
                current_row.append(href)
            except Exception as e:
                # If no link is found, append None or an empty string
                current_row.append(None)

            # Append this list to the result list
            result_list.append(current_row)

            # Increment the counter
            count += 1

    # Write the result to a CSV file
    with open('Website_rows.csv', mode='w', newline='', encoding='utf-8') as file:
        writer = csv.writer(file)
        
        # Write headers
        writer.writerow(['Count', 'Row Info', 'Row Link'])
        
        # Write each row
        for row in result_list:
            writer.writerow(row)

    print("CSV file created: Website_rows.csv")

# Example usage: extract_information for 10 pages

extract_information(10)

# In case you want to close the driver at the end
driver.quit()
```
This is a straightforward method for extracting data from websites in a concise and efficient manner. Always ensure that you comply with privacy regulations and avoid violating Personally Identifiable Information (PII) when handling profiles, addresses, or other sensitive data. If PII is involved, focus on extracting anonymous datasets. The goal of this practice is to identify trends and insights by quantifying data or automating time-consuming processes. 

Thank you for reviewing this project and I hope this application proves useful in your future research endeavors!
