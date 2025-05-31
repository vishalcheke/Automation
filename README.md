from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

# Setup ChromeDriver
driver = webdriver.Chrome()

try:
    # Step 1: Open the hotel booking application
    driver.get("https://example-hotel-booking.com")  # Replace with actual URL
    driver.maximize_window()

    # Step 2: Search for hotels in “New York” for April 10 - April 15
    WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.ID, "destination"))).send_keys("New York")
    driver.find_element(By.ID, "checkin").send_keys("2025-04-10")
    driver.find_element(By.ID, "checkout").send_keys("2025-04-15")
    driver.find_element(By.ID, "searchBtn").click()

    # Step 3: Select the first hotel from the search results
    WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.CSS_SELECTOR, ".hotel-card:first-child .book-button"))).click()

    # Step 4: Apply the coupon code "SUMMER25"
    WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.ID, "couponCode"))).send_keys("SUMMER25")
    driver.find_element(By.ID, "applyCoupon").click()

    # Step 5: Verify that the discount is applied correctly
    time.sleep(2)  # Can be replaced with wait
    discount_element = WebDriverWait(driver, 10).until(EC.visibility_of_element_located((By.ID, "discountMessage")))
    assert "Discount applied" in discount_element.text

    # Step 6: Proceed to checkout (do not complete payment)
    driver.find_element(By.ID, "checkoutBtn").click()
    print("Test completed successfully - coupon applied and checkout page reached.")

except Exception as e:
    print(f"Test failed due to: {e}")

finally:
    time.sleep(2)
    driver.quit()
