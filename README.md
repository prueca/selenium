# Selenium Automated Testing Suite

Selenium automates browser.

Note: With xampp, an outdated phpunit is installed by default. Follow these steps to override xampp phpunit:

- Go to xampp/php folder and delete two files phpunit (with no extension) and phpunit.bat.
- Go to xampp/php/PEAR dir and delete two folders PHPUnit and PHPUnit2.

##### Download and setup required files:

- Download gecko driver for firefox from https://github.com/mozilla/geckodriver/releases to project directory
- Download chrome driver for chrome from http://chromedriver.chromium.org/downloads to project directory
- Download and install java from https://www.java.com/en/download/
- Download selenium standalone server from https://www.seleniumhq.org/download/ to project directory
- Run selenium standalone server

###### Run selenium server

```bash
java -jar selenium-server-standalone-[version].jar
```

###### Run selenium server with specified gecko driver


```bash
java -jar -Dwebdriver.firefox.driver=[path/to/geckodriver.exe] selenium-server-standalone-[version].jar
```

###### Run selenium server with specified chrome driver

```bash
java -jar -Dwebdriver.firefox.driver=[path/to/chromedriver.exe] selenium-server-standalone-[version].jar
```

### PHPUnit-Selenium

- composer require --dev phpunit/phpunit-selenium
- Copy sample php test case and run

```php
require(__DIR__ . '/vendor/autoload.php');

class WebTest extends PHPUnit_Extensions_Selenium2TestCase
{
    protected function setUp()
    {
        $this->setBrowser('chrome');
        $this->setBrowserUrl('https://www.autopartswarehouse.com/');
    }

    public function testCase()
    {
        $this->url('https://www.autopartswarehouse.com/');

        $input = $this->byId('search_text');
        $input->value('air filter');

        $form = $this->byId('Search');
        $form->submit();
        
        file_put_contents(__DIR__ . '/test.png', $this->currentScreenshot());
    }
}
```

```bash
./vendor/bin/phpunit [path/to/file.php]
```

### PHPUnit-FacebookWebDriver

- composer require --dev phpunit/phpunit
- composer require --dev facebook/webdriver
- Copy sample php test case and run

```php
require(__DIR__ . '/vendor/autoload.php');

use Facebook\WebDriver\Remote\RemoteWebDriver;
use Facebook\WebDriver\Remote\WebDriverCapabilityType;
use Facebook\WebDriver\WebDriverBy;
use PHPUnit\Framework\TestCase;

class WebTest extends TestCase
{
	protected $driver;
	protected $url = 'https://www.google.com';

	public function setUp(): void
	{
		$capabilities = array(WebDriverCapabilityType::BROWSER_NAME => 'chrome');
        	$this->driver = RemoteWebDriver::create('http://localhost:4444/wd/hub', $capabilities);
	}

	public function testBrowse()
    	{
        	$this->driver->get($this->url);
        	$input = WebDriverBy::cssSelector('input[name="q"]');
        	$this->driver->findElement($input)->sendKeys('test');
    	} 
}
```

```bash
./vendor/bin/phpunit [path/to/file.php]
```

### NodeJS-Selenium

- npm i selenium-webdriver
- Compy sample javascript test case and run

```javascript
const {Builder, By, Key, until} = require('selenium-webdriver');

(async () => {
  let driver = new Builder()
    .forBrowser('chrome')
    .usingServer('http://localhost:4444/wd/hub')
    .build();

  try {
    await driver.get('http://www.google.com/ncr');
    await driver.findElement(By.name('q')).sendKeys('webdriver', Key.RETURN);
    await driver.wait(until.titleIs('webdriver - Google Search'), 1000);
  } finally {
    await driver.quit();
  }
})();
```

```bash
node [path/to/file.js]
```
