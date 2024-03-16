from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Initialize Selenium WebDriver
driver = webdriver.Chrome()

# Open the application
driver.get("https://movie-reviews-psi.vercel.app/")

# UI Testing for Full Stack Application
try:
    # Wait for movie cards to be displayed
    movie_cards = WebDriverWait(driver, 10).until(
        EC.presence_of_all_elements_located((By.CLASS_NAME, "movie-card"))
    )
    assert len(movie_cards) > 0, "No movie cards found"

    # Navigate to movie review page on card click
    movie_cards[0].click()
    assert "movie" in driver.current_url, "Failed to navigate to movie review page"

    # Add a new movie
    driver.find_element_by_xpath("//button[text()='Add new movie']").click()
    title_input = driver.find_element_by_id("name")
    title_input.send_keys("New Movie")
    driver.find_element_by_xpath("//button[text()='Create Movie']").click()

    # Verify the new movie in the list
    new_movie_card = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.XPATH, f"//div[@class='movie-card'][contains(text(),'New Movie')]"))
    )
    assert new_movie_card.is_displayed(), "Newly added movie not found in the list"
