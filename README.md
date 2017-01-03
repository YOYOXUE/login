# login
#utf-8


import request 
import re
import time
import os.path
 
 # Request headers
 agent='Mozilla/5.0(Windows NT 5.2;rv:33.0) Gecko/20120909 Firefox/33.0
 headers={
      "Host":"www.zhihu.com/",
      “Referer":"https://www.zhihu.com/",
      "User-Agent":agent
}

#cookie information
session=request.session()
session.cookies=cookielib.LWPCookieJar（filename='cookies')
try:
    session.cookies.load(ignore_discard=True)
except:
    print("cookie unloading")
    
def get_xsrf()
    #xsrf: varaible parameter
    index_url="https://www.zhihu.com"
    index_page=session.get(index_url, headers=headers)
    html = index_page.text
    pattern = r'name="_xsrf" value="(.*?)"'
    _xsrf = re.findall(pattern, html)
    return _xsrf[0]
    
#get verification code
def get_captcha():
    t=str(int(time.time()*1000))
    captcha_url = 'https://www.zhihu.com/captcha.gif?r=' + t + "&type=login"
    r = session.get(captcha_url, headers=headers)
    with open('captcha.jpg', 'wb') as f:
        f.write(r.content)
        f.close()
    try:
        im=Image.open('captcha.jpg')
        im.show()
        im.close()
    except:
        printprint(u'Please %s findcaptcha.jpg and input it' % os.path.abspath('captcha.jpg'))
    captcha = input("please input the captcha\n>")
    return captcha
    
def isLogin():
    url= "https://www.zhihu.com/settings/profile"
    login_code = session.get(url, headers=headers, allow_redirects=False).status_code
     if login_code == 200:
         return True
     else:
         return False
         
def login(secret, account):
    #if the user account is his cellphone
     if re.match(r"^1\d{10}$", account):
         print("login with phone number \n")
         post_url = 'https://www.zhihu.com/login/phone_num'
         postdata = {
             '_xsrf': get_xsrf(),
             'password': secret,
             'remember_me': 'true',
             'phone_num': account,
         }
     else:
         if "@" in account:
              print("use email address \n")
         else：
              print("Your email address is incorrect")
              return 0
         post_url = 'https://www.zhihu.com/login/email'
         
