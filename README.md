# D49-100DaysOfCode
Hey, this is my custom solution to Angela Yu's Day 49 100DaysOfCode Challenge. I spent a bit of time on it and you're likely to find errors but it works and goes a little beyond what was required.
While you're here check out my twitter page, and feel free to message me if you're curious about anything or need help!

from time import sleep
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.common.exceptions import NoSuchElementException

def click_element(driver, xpath, error_msg, sleep_time=5):
    try:
        driver.find_element(By.XPATH, xpath).click()
        sleep(sleep_time)
    except NoSuchElementException:
        print(f"{error_msg}")

def main():
    # Your LinkedIn credentials
    EMAIL = "your_email@example.com"  # Replace with your LinkedIn email
    PASSWORD = "your_password"         # Replace with your LinkedIn password

    driver = webdriver.Chrome()
    driver.get("https://www.linkedin.com/login")

    # XPaths with descriptive labels
    xpaths = {
        'login_button': '//*[@id="organic-div"]/form/div[4]/button',
        'jobs_nav_button': '//*[@id="global-nav"]/div/nav/ul/li[3]/a',
        'show_all_jobs': '/html/body/div[6]/div[3]/div/div[3]/div/div/main/div/div/div[1]/div[1]/div/div/div/section/div[2]/a',
        'search_box': '/html/body/div[6]/header/div/div/div/div[2]/div[2]/div/div/input[1]',
        'easy_apply_filter': '//*[@id="searchFilter_applyWithLinkedin"]',
        'apply_button': '//*[@id="jobs-apply-button-id"]'
    }

    # Application form XPaths
    form_button_xpaths = {
        'first_next': '/html/body/div[4]/div/div/div[2]/div/div[2]/form/footer/div[2]/button',
        'second_next': '/html/body/div[4]/div/div/div[2]/div/div[2]/form/footer/div[2]/button[2]',
        'third_next': '/html/body/div[4]/div/div/div[2]/div/div[2]/form/footer/div[2]/button[2]',
        'review_button': '/html/body/div[4]/div/div/div[2]/div/div[2]/form/footer/div[2]/button[2]',
        'submit_button': '/html/body/div[4]/div/div/div[2]/div/div[2]/div/footer/div[3]/button[2]',
        'done_button': '/html/body/div[4]/div/div/div[3]/button',
        'close_button': '/html/body/div[4]/div/div/button',
        'discard_button': '/html/body/div[4]/div[2]/div/div[3]/button[1]'
    }

    # Login
    driver.find_element(By.NAME, "session_key").send_keys(EMAIL)
    driver.find_element(By.NAME, "session_password").send_keys(PASSWORD)
    driver.find_element(By.XPATH, xpaths['login_button']).click()
    sleep(20)

    # Navigate to jobs and apply filters
    click_element(driver, xpaths['jobs_nav_button'], "1.Cannot find Jobs navigation button")
    click_element(driver, xpaths['show_all_jobs'], "2.Cannot find Show all jobs button")

    # Search for jobs
    try:
        search_box = driver.find_element(By.XPATH, xpaths['search_box'])
        search_box.send_keys("Software Engineer")  # You can modify this search term
        search_box.send_keys(Keys.ENTER)
        sleep(5)
    except NoSuchElementException:
        print("3.Cannot find search box")

    # Apply filters and start application
    click_element(driver, xpaths['easy_apply_filter'], "4.Cannot find Easy Apply filter")
    sleep(10)
    click_element(driver, xpaths['apply_button'], "5.Cannot find Apply button", 10)

    # Application process
    for button_name, xpath in form_button_xpaths.items():
        try:
            click_element(driver, xpath, f"Cannot find {button_name}")
        except:
            print("Multi Form")
            click_element(driver, form_button_xpaths['close_button'], "Cannot find Done button")

    sleep(120)

if __name__ == "__main__":
    main()
