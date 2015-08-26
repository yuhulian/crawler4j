# Configuring Crawler #
As of version 3.0, the controller class has a mandatory parameter of type [CrawlConfig](http://code.google.com/p/crawler4j/source/browse/src/main/java/edu/uci/ics/crawler4j/crawler/CrawlConfig.java). Instances of this class can be used for configuring crawler4j. The following sections describe some details of configurations.

## Crawl depth ##
By default there is no limit on the depth of crawling. But you can limit the depth of crawling. For example, assume that you have a seed page "A", which links to "B", which links to "C", which links to "D". So, we have the following link structure:

A -> B -> C -> D

Since, "A" is a seed page, it will have a depth of 0. "B" will have depth of 1 and so on. You can set a limit on the depth of pages that crawler4j crawls. For example, if you set this limit to 2, it won't crawl page "D". To set the maximum depth you can use:
```
crawlConfig.setMaxDepthOfCrawling(maxDepthOfCrawling);
```

## Maximum number of pages to crawl ##
Although by default there is no limit on the number of pages to crawl, you can set a limit on this:
```
crawlConfig.setMaxPagesToFetch(maxPagesToFetch);
```

## Politeness ##
crawler4j is designed very efficiently and has the ability to crawl domains very fast (e.g., it has been able to crawl 200 Wikipedia pages per second). However, since this is against crawling policies and puts huge load on servers (and they might block you!), since version 1.3, by default crawler4j waits at least 200 milliseconds between requests. However, this parameter can be tuned:
```
crawlConfig.setPolitenessDelay(politenessDelay);
```

## Proxy ##
Should your crawl run behind a proxy? If so, you can use:
```
crawlConfig.setProxyHost("proxyserver.example.com");
crawlConfig.setProxyPort(8080);
```
If your proxy also needs authentication:
```
crawlConfig.setProxyUsername(username); 
crawlConfig.getProxyPassword(password);
```

## Resumable Crawling ##
Sometimes you need to run a crawler for a long time. It is possible that the crawler terminates unexpectedly. In such cases, it might be desirable to resume the crawling. You would be able to resume a previously stopped/crashed crawl using the following settings:
```
crawlConfig.setResumableCrawling(true);
```

However, you should note that it might make the crawling slightly slower.

## User agent string ##
User-agent string is used for representing your crawler to web servers. See [here](http://en.wikipedia.org/wiki/User_agent) for more details. By default crawler4j uses the following user agent string:
```
"crawler4j (http://code.google.com/p/crawler4j/)"
```
However, you can overwrite it:
```
crawlConfig.setUserAgentString(userAgentString);
```