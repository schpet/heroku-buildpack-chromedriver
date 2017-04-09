# heroku-buildpack-chromedriver

### resources

- https://github.com/heroku/heroku-buildpack-google-chrome
- https://github.com/masamitsu-konya/heroku-buildpack-chromedriver

### notes

if you're using this in addition to [heroku's chrome buildpack][1]:

currently, chrome is installed as "google-chrome-unstable" and chromedriver cannot
find it 😦. this is how i was able to set up selenium to find it in ruby:

```rb
# CHROME_PATH is /app/.apt/usr/bin/google-chrome-unstable
Capybara.register_driver :selenium do |app|
  if ENV["CHROME_PATH"]
    Capybara::Selenium::Driver.new(
      app,
      browser: :chrome,
      desired_capabilities: Selenium::WebDriver::Remote::Capabilities.chrome(
        "chromeOptions" => {
          binary: ENV.fetch("CHROME_PATH")
        }
      )
    )
  else
    Capybara::Selenium::Driver.new(app, browser: :chrome)
  end
end

Capybara.default_driver = :selenium
```

[1]: https://github.com/heroku/heroku-buildpack-google-chrome
