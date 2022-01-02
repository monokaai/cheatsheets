## [Official API reference](https://selenium-python.readthedocs.io/api.html)
## [Quick reference in Japanese](https://www.seleniumqref.com/)
<!-- https://tanuhack.com/selenium/#WebDriver) -->


1. Major modules
    ```python
    from selenium import webdriver
    from selenium.webdriver.chrome.options import Options
    from selenium.webdriver.support.select import Select
    from selenium.webdriver.common.by import By
    from selenium.webdriver.common.keys import Keys
    from selenium.webdriver.common.alert import Alert
    from selenium.webdriver.support.ui import WebDriverWait
    from selenium.webdriver.support import expected_conditions as EC
    from selenium.common.exceptions import TimeoutException
    ```

2. Options
    ```python
    options = Options()
    options.add_argument('--disable-gpu')
    options.add_argument('--disable-extensions')
    options.add_argument('--proxy-server="direct://"')
    options.add_argument('--proxy-bypass-list=*')
    options.add_argument('--start-maximized')
    options.add_argument('--kiosk') # window maximization
    options.add_argument('--headless')
    ```

3. Entire page manipulation
    ```python
    driver.get(url)
    title = driver.title
    url = driver.current_url
    driver.back()
    driver.forward()
    driver.refresh()
    driver.close()
    driver.quit()
    driver.save_screenshot(screenshot_path) # end with file.ext
    Alert(driver).accept()
    Alert(driver).dismiss()
    # Switch to latest window (target="_blank")
    handle_array = driver.window_handles
    driver.switch_to.window(handle_array[-1])
    driver.find_element_by_id()
    driver.find_element_by_class_name()
    driver.find_element_by_name()
    driver.find_element_css_selector()
    driver.find_element_by_xpath()
    ```

4. CSS selectors
    ```css
    tag: h2
    id: #title
    class: .red
    attribute: a[target]
    attr-value-matches: input[type="text"]
    attr-starts-or-ends: p[class^="en"], p[class$="n"]
    attr-includes: p[class*="en"]
    selector-and: .color.blue
    selector-multiple: .red, .blue
    selector-in: nav ul .example
    child-only: .example > p
    next : .example + li
    before : .example ~ li
    first: ul li:first-child
    last: ul li:last-child
    nth: ul li:nth-child(3), nth-child(2n-1)
    not: ul > li:not(.blue)
    ```
5. Elements manipulation
    ```python
    element = driver.find_element_by_css_selector(selector)
    element.text
    element.get_attribute('attribute-name')
    element.send_keys('input-text')
    element.clear()
    # if element.clear doesn't work:
    for _ in range(len(element.text)):
        element.send_keys(Keys.BACK_SPACE)

    # "Click" alternatives
    element.click()
    driver.execute_script('arguments[0].click();', element)
    element.send_keys(Keys.ENTER)

    # special keys
    element.send_keys(Keys.BACK_SPACE)
    element.send_keys(Keys.ESCAPE)
    element.send_keys(Keys.TAB)

    # deselect_by_ are also available
    Select(element).select_by_value('value')
    Select(element).select_by_index('index')
    Select(element).select_by_visible_text('text')
    Select(element).deselect_all()

    # check status
    element.is_displayed()
    element.is_enabled()
    element.is_selected()

    # drag and drop
    src = driver.find_element_by_name("source")
    tgt = driver.find_element_by_name("target")
    actions.drag_and_drop(src, tgt).perform()
    ```

6. Waiting
    ```python
    driver.implicitly_wait(10)

    element = WebDriverWait(driver, 30).until(
	EC.visibility_of_element_located((By.CSS_SELECTOR, selector)))

    # wait randomly
    import numpy as np
    import time
    # 10.000~12.000 sec
    wait_time = float('{:.3f}'.format(np.random.rand()*2+10))
    time.sleep(wait_time)
    ```

7. Use BeautifulSoup
    ```python
    from bs4 import BeautifulSoup
    ...
    html = driver.page_source
    soup = BeautifulSoup(html, 'lxml')
    selector = '{{css-selector}}'
    text = soup.select_one(selector).get_text()
    attr = soup.select_one(selector).get('attr')
    texts = [i.get_text() for i in soup.select(selector)]
    attrs = [i.get('attr') for i in soup.select(selector)]
    ```

8. 2FA
   ```bash
    $ oathtool --totp --base32 account-key
   ```
    ```python
    import subprocess

    two_step_authentication = ['oathtool', '--totp', '--base32', 'account-key']
    SecondLoginPass = re.findall(r'\d+', subprocess.check_output(two_step_authentication).decode('utf-8'))
    print(SecondLoginPass[0])
    print(subprocess.check_output(two_step_authentication))
    ```