# Python_Automation_Project
Applying Automation for Data Extraction
- Transitioning from AI bots to Python automation resulted in eliminating all associated costs with data extraction, leading to significant budget savings for the organization.
- The implementation of validation checks in the automation scripts maintains the overall data quality and reliability.

## Web Driver and Selenium Packages
Ensure that the ChromeDriver version matches the version of Chrome, Firefox, or any other browser you're using; otherwise, the driver will not function properly. Additionally, you'll need the Selenium package, which can be downloaded using the link below.

https://www.selenium.dev/

I use Jupyter Notebooks with Anaconda Navigator, which conveniently includes the Selenium package. You can easily find and install it from the Anaconda library. If you're interested in using the same setup, you can download Anaconda Navigator here:

https://www.anaconda.com/

Install the Driver is necessary. 

![Alt text](https://github.com/FrankCoRa/EDA_Waykitech/blob/main/Boxplot_Views.png)
https://developer.chrome.com/docs/chromedriver/downloads
To avoid conflicts running your code , you must have the same version of your driver as your current explorer ( in my case Chrome)

## Web Objective
Ensure you thoroughly inspect the website you plan to extract data from, as each site has unique class names, buttons, spans, IDs, and other elements. If you switch to a different website, these references will vary, requiring you to re-inspect and adjust your extraction script accordingly. Additionally, when automating clicks using JavaScript, modifications may be necessary. Therefore, it's crucial to be 100% certain of the website setup before executing any automation.

## Automation Project Extract
While I am unable to share the complete automation script due to restrictions, I will provide the initial segment and core components of the process. I'll aim to make the explanation as interactive and engaging as possible.

# Automation Code (Python & Java Script)

First of All, Your selenium package has to be updated to the last version. In the case of Anaconda , Go to enviroments> Not Installed > Search Packages (Type Selenium), then install.

After installed the packages , Open Jupyter Notebooks and run the following code to update selenium to the latest version.

```r
pip install --upgrade selenium
```
## Importing Libraries
```r
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from selenium.common.exceptions import TimeoutException, NoSuchElementException
import time
import csv
import random
options = Options()

# Set a custom user-agent to mimic a real browser
options.add_argument("user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36")

```
## Web Driver
```r
# Path to your ChromeDriver executable
chromedriver_path = "###"
```In Here '###' goes the location path where you installed the driver, DONT FORGET the driver has to be the same version of your browser.

# Create ChromeOptions instance
chrome_options = Options()

# Add the --whitelisted-ips argument
# To allow all IPs, use an empty string
chrome_options.add_argument("--whitelisted-ips=")

# Alternatively, to allow specific IPs, use a comma-separated list
# chrome_options.add_argument("--whitelisted-ips=192.168.1.5,10.0.0.1")

# Create a Service instance
service = Service(executable_path=chromedriver_path)

# Create a new Chrome driver instance with the options
driver = webdriver.Chrome(service=service, options=chrome_options)

# Use the driver
driver.get("##YOUR_SIGN_IN_HOMEPAGE_LINK##")

```
## Insert Account
```r
# Insert email, In this case the XPATH name is 'session_key' and 'session_password'.In your case you have to inspect the Xpath required for your input.
# I again recommend using a test account to safeguard your personal information
# Insert email
email_input = WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.XPATH, "//input[@name='session_key']"))
)
email_input.send_keys("##YOUR USER ID##")

# Insert password
email_password = WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.XPATH, "//input[@name='session_password']"))
)
email_password.send_keys("## YOUR PASWORD ##")

# Click 'Sign in' button
signin_button = WebDriverWait(driver, 10).until(
    EC.element_to_be_clickable((By.XPATH, "//button[@type='submit']"))
)
signin_button.click()

# Wait for navigation (adjust time if needed)
time.sleep(3)
```
## Extract Function
```r

# Define the function to extract profiles
def extract_profiles(n):
    # Initialize result list
    result_list = []
    count = 0

    try:
        for i in range(1, n + 1):
            print(f"Processing page {i}...")

            # Add a dynamic wait before opening the next page to avoid rate-limiting
            wait_time = 8 + i % 5  # Base wait of 8 seconds, with a small variation
            time.sleep(wait_time)

            # Open the URL for the specific page
            driver.get(f"### YourLink&page={i}&sid=TWP ###")
            ``` This algoritm only works with number of pages, try to find a link with page number to make feasible the iteration for the function ```

            try:
                # Wait until profile containers with role="list" are present
                all_class = WebDriverWait(driver, 20).until(
                    EC.presence_of_all_elements_located((By.XPATH, '//*[@class="### TYPE OF CLASS OF THE PATH ###"]'))
                )
            ```To locate the class of the path do right click in the desired page > Inspector > Select the Area that you want to extract > Copy the type of class```

            except Exception as e:
                print(f"Error waiting for profiles on page {i}: {e}")
                continue  # Skip to the next page if profiles are not found

            # Extract information for each profile
            for j in all_class:
                try:
                    # Split the text by newlines and prepare the person's data
                    text_split = j.text.split("\n")
                    current_person = [str(count)] + text_split

                    # Try extracting the profile link
                    try:
                        link_element = j.find_element(By.XPATH, './/a[contains(@class, "### TYPE OF CLASS OF THE PATH ###")]')
                        href = link_element.get_attribute("href")
                    except Exception as e:
                        print(f"Error extracting profile link: {e}")
                        href = None
                    ```To locate the class of the path do right click in the desired page > Inspector > Select the Link Area that you want to extract > Copy the type                     of class```

                    current_person.append(href)
                    result_list.append(current_person)
                    count += 1
                except Exception as e:
                    print(f"Error processing a profile: {e}")

        # Write the results to a CSV file
        with open('extracted_profiles.csv', mode='w', newline='', encoding='utf-8') as file:
            writer = csv.writer(file)
            writer.writerow(['Count', 'Profile Info', 'Profile Link'])
            writer.writerows(result_list)

        print("CSV file created: extracted_profiles.csv")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        # Ensure the driver quits even if an error occurs
        driver.quit()
extract_profiles(5) 
```
This is a straightforward method for extracting data from websites in a concise and efficient manner. Always ensure that you comply with privacy regulations and avoid violating Personally Identifiable Information (PII) when handling profiles, addresses, or other sensitive data. If PII is involved, focus on extracting anonymous datasets. The goal of this practice is to identify trends and insights by quantifying data or automating time-consuming processes. 

Thank you for reviewing this project and I hope this application proves useful in your future research endeavors!
