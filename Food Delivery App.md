# AUTOMATED TEST SCRIPT

```from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager
import time

service = Service(ChromeDriverManager().install())
driver = webdriver.Chrome(service=service)
driver.maximize_window()
try:
    driver.get("https://pwiddy.interview.tisostudio.com/")
    print("Opened the website")
    time.sleep(5)

    search_box = WebDriverWait(driver, 15).until(
        EC.presence_of_element_located((By.XPATH, "//input[@placeholder='Search for restaurants...']"))
    )
    search_box.send_keys("West, Mayer and Wintheiser")
    print("Entered restaurant name in search")
    time.sleep(3)
    restaurant = WebDriverWait(driver, 15).until(
        EC.element_to_be_clickable((By.XPATH, "//h2[contains(text(),'West, Mayer and Wintheiser')]"))
    )
    restaurant.click()
    print("Selected the restaurant")

    time.sleep(5)
    items = driver.find_elements(By.XPATH, "//button[contains(text(),'Add to Cart')]")
    if not items:
        raise Exception("No items found to add to cart")
    for i in range(min(2, len(items))):
        items[i].click()
        print(f"Item {i+1} added to cart")
        time.sleep(2)

    cart_button = WebDriverWait(driver, 10).until(
        EC.element_to_be_clickable((By.XPATH, "//a[@href='/cart']"))
    )
    cart_button.click()
    print("Navigated to cart")

    address_input = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.XPATH, "//input[@name='address']"))
    )
    address_input.send_keys("123 Main Street, Hyderabad")
    print("Entered delivery address")

    cash_payment = WebDriverWait(driver, 10).until(
        EC.element_to_be_clickable((By.XPATH, "//input[@value='CASH']"))
    )
    cash_payment.click()
    print("Selected cash as payment method")

    checkout_button = WebDriverWait(driver, 10).until(
        EC.element_to_be_clickable((By.XPATH, "//button[contains(text(),'Place Order')]"))
    )
    checkout_button.click()
    print("Submitted the order")
    WebDriverWait(driver, 10).until(EC.title_contains("Order"))
    print("Order completed successfully")
except Exception as e:
    print("Test failed:", e)
finally:
    driver.quit()
```
