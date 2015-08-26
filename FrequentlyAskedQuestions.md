# Frequently Asked Questions #

  * **Should I keep track of visited URLs to make sure that crawler4j doesn't crawl them twice?** No, crawler4j manages that.

  * **I have noticed that the crawler doesn't see that http://some.url.com/sub the same as http://some.url.com/sub/ and crawls them separately.** These URLs are technically different. The first one can refer to a file called "sub". The second one can refer to http://some.url.com/sub/index.htm or something similar. So, web crawlers consider them as different. But if you're crawling a domain and you're sure that in that domain these two URLs are the same then you can add the required logic to your code.

  * **I see "I/O exception (org.apache.http.NoHttpResponseException) caught when processing request: The target server failed to respond" in the logs. Is this an error?** No. This means that the server that is serving the page you have requested has not been able to process your request. This can be a transient problem that might happen on the server because of the load on server. crawler4j retries several times for each URL for which this happens. But generally, in each crawl, there is a percentage of the pages that might not be downloaded because of the network or server problems. So, don't expect to see 100% of the requested pages in your results.

  * **How to change the regular expression in BasicCrawler to exclude also all pages containing the word: "aboutus" (for the example) ?** This is a regular expression understanding issue.

The current regular expression in BasicCrawler (before your additions) means that it will recognize any url which ends (the $ sign means the end of the url) with one of those extensions.

You want to add another regular expression condition which means [url which ends with the above list of extensions OR where the file name contains the following word: "aboutus"](any.md)


I recommend to try to find your new regular expression using the following tool:
http://regexpal.com/

put in the first text field the current regular expression which is:
.**(\.(css|js|bmp|gif|jpe?g|doc|png|xls|xlsx|tiff?|mid|mp2|mp3|htm|csv|pptx|ppt|mp4|wav|avi|mov|mpeg|ram|m4v|rm|smil|wmv|swf|wma|rar|gz))$**

Now in the second box put something which will be caught in this regular expression like: avi.csv

You see that it gets yellow as the regular expression will catch it.


Now put your URLs which you want to be caught there (like aboutus.php) and try to rewrite the first textbox with a regular expression which will catch it.

Now you have two options, you can try to tweak the original regexp to include also this case, or you can create a brand new regexp to include only your personal case and then in the BasicCrawler create two patterns and in "shouldVisit" include both like: return !FILTERS.matcher(href).matches() && !NEW\_FILTER.matcher(href).matches()