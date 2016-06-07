Notes - How to use onedrive SDK 
====


https://github.com/OneDrive/onedrive-sdk-python/issues/8

The `GetAuthCodeServer` module does assume that there is a usable browser. It is not possible at this time to do initial app authorization without a browser. Here's how I would work around that.

1. Use the script below on a computer with a web browser to get through the initial code acquisition. That's the only part where a browser is required.
	* Make sure that you include `wl.offline_access` in your scopes requested. That will give you a `refresh token`, which you will need later.
* Transfer the `code` in the `response` from the auth server over to your Pi. The best way to do that will depend on how you develop on your Pi.
* Use the code in `client.auth_provider.authenticate()`. After that call, your client is authenticated, and any calls to the API should succeed.
* Use `auth_provider.save_session()` to save the session information for later. Then, at the beginning of every session, just use `load_session()` and `refresh_token()` to get new tokens. You'll never need to use a browser again!

A caution on step (4): the default implementation of the Session object is NOT safe for use anywhere other than development. It saves your auth key to a `.pickle` file, which is basically like saving your password into a .txt file. If you want to use this in a more serious application, I would recommend subclassing the Session object to use a safer form of storage.

```
import onedrivesdk
from onedrivesdk.helpers import GetAuthCodeServer

if __name__ == "__main__":
    client_id = "{your_client_id}"
    redirect_uri = "http://localhost:8080/"

    client = onedrivesdk.get_default_client(client_id=client_id,
                                            scopes=['onedrive.readwrite',
                                                    'wl.signin',
                                                    'wl.offline_access'])

    auth_url = client.auth_provider.get_auth_url(redirect_uri)

    # Block thread until we have the code
    code = GetAuthCodeServer.get_auth_code(auth_url, redirect_uri)
    print("Token available as 'code' local var, and here:\n")
    print(code)   
```


```
import onedrivesdk
from onedrivesdk.helpers import GetAuthCodeServer

client_id = "07f64fba-b697-4867-8290-c263a123c49b"
redirect_uri = "https://localhost:8080/"
client = onedrivesdk.get_default_client(client_id=client_id,
                                        scopes=['onedrive.readwrite',
                                                'wl.signin',
                                                'wl.offline_access'])
auth_url = client.auth_provider.get_auth_url(redirect_uri)
# Block thread until we have the code
code = GetAuthCodeServer.get_auth_code(auth_url, redirect_uri)
print("Token available as 'code' local var, and here:\n")
print(code)  
```

Getting authentication access from onedrive without browser interaction can be done with Web driver phantomjs and Selenium.

```
import onedrivesdk
from selenium import webdriver

#Onedrive Access code from control panel 
redirect_uri = "https://login.live.com/oauth20_desktop.srf"

#replace this value from your one drive developer panel.
client_secret = "TofmeJ5qKQMOSNRapWg5vsz"
client_id_str = "07f64fba-b697-4867-8290-c263a123c49b"

user_name = 'vikas.ymca@gmail.com	'
password = 'Pa$$w0rd'

client = onedrivesdk.get_default_client(client_id=client_id_str,
scopes=['wl.signin',
'wl.offline_access',
'onedrive.readwrite'])
auth_url = client.auth_provider.get_auth_url(redirect_uri)
driver = webdriver.PhantomJS() # 'phantomjs')


"""
driver = webdriver.Chrome('chromedriver')
driver.set_window_size(1, 1)
"""

driver.get(auth_url)
driver.implicitly_wait(15)

login_field = driver.find_element_by_name("loginfmt")
password_field = driver.find_element_by_name("passwd")
sign_btn = driver.find_element_by_id("idSIButton9")

#Fill user name and password

login_field.send_keys(user_name)
password_field.send_keys(password)
sign_btn.click()
element = driver.find_element_by_name('ucaccept') 
element.click()

#parse URL to get code and close the browser

tokens = driver.current_url.split('=')
driver.quit()

print(tokens[1].split('&')[0])

access_code = tokens[1].split('&')[0]
print("access code is here ", access_code)
```



- OneDrive API RESTful programming in Java – part 1: registering and authorizing http://www.tjeerd.net/2014/08/23/onedrive-api-restful-programming-in-java/

Provides detail about refresh tokens

- https://dev.onedrive.com/auth/msa_oauth.htm 
