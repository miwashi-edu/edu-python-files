# edu-python-files

## Prepare

```bash
cd ~
cd ws
mkdir python_files && cd python_files
pip install selenium requests beautifulsoup4 requests-html pyppeteer

#Important to go to https://chromedriver.storage.googleapis.com and check the version is compatible with installed chrome
curl -LO https://chromedriver.storage.googleapis.com/114.0.5735.90/chromedriver_linux64.zip
curl -LO https://chromedriver.storage.googleapis.com/114.0.5735.90/chromedriver_mac64.zip
curl -LO https://chromedriver.storage.googleapis.com/114.0.5735.90/chromedriver_mac_arm64.zip
curl -LO https://chromedriver.storage.googleapis.com/114.0.5735.90/chromedriver_win32.zip
unzip chromedriver_linux64.zip # Uzipping for Kali linux
chmod +x ./chromedriver
```

## Scraping Static

```bash
cd ~
cd ws
cd python_files
echo '#!'"$(which python3)" >  scraping
chmod +x scraping
cat >> scraping << 'EOF'
import requests
from bs4 import BeautifulSoup

url = "https://share.netresec.com/s/7qgDSGNGw2NY8ea"
response = requests.get(url)
html_content = response.text

# Parse HTML with BeautifulSoup
soup = BeautifulSoup(html_content, 'html.parser')

# Find all <a> tags with 'href' attribute
all_urls = [a['href'] for a in soup.find_all('a', href=True)]

# Filter URLs to include only those that start with "maccdc"
file_urls = list(filter(lambda u: 'maccdc' in u, all_urls))

# Download each file
filename = ""
for url in file_urls:
    try:
        print(f"Attempting to download {url}")
        response = requests.get(url)
        filename = url.split('/')[-1]

        # Write the file content
        with open(filename, 'wb') as file:
            file.write(response.content)
        print(f"Successfully downloaded {filename}")
    except Exception as e:
        print(f"An error occurred while downloading or writing the file {filename}: {e}")
        continue  # Skip to the next file

# Add additional code here for unpacking and joining files if needed
EOF
```

## Scraping Dynamic -- Selenium

```bash
cd ~
cd ws
cd python_files
echo '#!'"$(which python3)" >  scraping
chmod +x scraping
cat >> scraping << 'EOF'
from selenium import webdriver
from bs4 import BeautifulSoup
import time
import requests

# Selenium setup
url = "https://share.netresec.com/s/7qgDSGNGw2NY8ea"
driver = webdriver.Chrome('/Users/miwa/ws/python_files/chromedriver')  # Replace with the correct path to your Chromedriver
driver.get(url)

# Wait for JavaScript to load the content
time.sleep(5)  # Adjust time as necessary

# Fetching the HTML content after JavaScript execution
html_content = driver.page_source
driver.quit()

# Parse HTML with BeautifulSoup
soup = BeautifulSoup(html_content, 'html.parser')

# Find all <a> tags with 'href' attribute
all_urls = [a['href'] for a in soup.find_all('a', href=True)]
print(f"Scraped {len(all_urls)} urls!")

# Filter URLs to include only those that contain "maccdc2012"
file_urls = list(filter(lambda u: 'maccdc2012' in u, all_urls))
print(f"Found {len(file_urls)} urls!")

# Download each file
for url in file_urls:
    try:
        print(f"Attempting to download {url}")
        response = requests.get(url)
        filename = url.split('/')[-1]

        # Write the file content
        with open(filename, 'wb') as file:
            file.write(response.content)
        print(f"Successfully downloaded {filename}")
    except Exception as e:
        print(f"An error occurred while downloading or writing the file {filename}: {e}")
        continue  # Skip to the next file
EOF
```
