## You can now crawl password protected pages using Crawler4j ##



# Introduction #

The way Crawler4J does it is by logging into the pages which have the username/password authentication, then saving the cookie so when the crawler tries to crawl one of those pages, it will already have it's cookie and it will have no problem crawling those (not anymore) protected pages.


# Details #

How should you setup crawler4j to crawl those pages?

Super simple, just add an AuthInfo instance into your crawlConfig instance as follows (in the following example I will add **two** login pages with their credentials, one for a "FORM" based login, the second for a "BASIC" based login, so when crawler4j will try to crawl either one of those two, it will already have their cookie and the crawling will go smooth):

**The Following Lines Should be in your CrawlController**

```
AuthInfo authInfo1 = new FormAuthInfo("USERNAME_1", "PASSWORD_1", "FULL_LOGIN_URL_1", "USERNAME_NAME_ATTRIBUTE_IN_LOGIN_FORM_1", "PASSWORD_NAME_ATTRIBUTE_IN_LOGIN_FORM_1");
    
AuthInfo authInfo2 = new BasicAuthInfo("USERNAME_2", "PASSWORD_2", "FULL_LOGIN_URL_2");
```

Now I will just add them to the crawlConfig:
```
config.addAuthInfo(authInfo1);
config.addAuthInfo(authInfo2);
```


That's it, when the crawler will start, it will attempt to login to both sites, and only after that it will begin the regular crawling.

Please note that we have currently implemented FORM and BASIC authentications







One small note, you should add to your "shouldVisit()" method, a limitation not to crawl the **logout** links, because if the crawler will crawl those, it will destroy the cookie and you won't be able to continue crawling your password protected site