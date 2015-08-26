The following tutorial assumes that you will be using an IDE. These instructions are tailored for the Eclipse IDE, but the procedures should be similar for `NetBeans` users. (IntelliJ and other IDE users are on your own but hopefully it won't be too different.)

**Setup**
  1. Download the latest crawler4j-x.x.zip file (contains crawler4j-x.x.jar) where x represents the current revision major and minor numbers. The zip file also contains a log4j.properties file that you'll need later to properly create a log4j appender.
  1. Download the latest crawler4j.dependencies.zip (contains multiple 3rd party jars).
  1. Extract the contents of both zip files to a convenient folder. There are lots of jars in the dependency file, so you'll probably want to put the extracted files into their own folder.
  1. In Eclipse, create a new project, such as `WebcrawlerTest`.
  1. Right click on your new web crawler project and select the Build Path option. Select the “Add External Archives” option and select ALL of the jar files that you extracted from the zip files.
  1. Your project should now show more than 15 jar files under Referenced Libraries.
  1. The crawler4j-x.x.jar doesn't include a “main” function, so create a Java class that has one.
  1. As a minimum, you'll need lines similar to the following ones (and their matching import statements):
```
public static void main(String[] args) {
	CrawlConfig crawlConfig = new CrawlConfig();
	crawlConfig.setCrawlStorageFolder("C:\\asp\\crawler4jStorage");
	System.out.println(crawlConfig.toString());
	PageFetcher pageFetcher = new PageFetcher(crawlConfig);
	RobotstxtConfig robotstxtConfig = new RobotstxtConfig();
	RobotstxtServer robotstxtServer = new RobotstxtServer(robotstxtConfig, pageFetcher);
	try {
		CrawlController crawlController = new CrawlController(crawlConfig,
			pageFetcher, robotstxtServer);
```

**(procedure continues even though the numbering reset.)**
  1. The System.out.println statement is optional, but it allows you to see how your webcrawler is configured.
  1. The crawlconfig.setCrawlStorageFolder() function must be used to set the storage folder that will be used when the webcrawler is running. A “frontiers” folder is added to your storage folder and some lock files are stored in it, make sure the permissions for your storage folder are set to allow frontiers to be added and its contents deleted. If the webcrawler gets stuck and you don't shut it down properly, you won't be able to start a new webcrawler until the stuck one gets shut down. You may have to exit Eclipse to get the web crawler to shut down properly.
  1. The crawlController throws Exception, so you'll need to put it into a try-catch group.
  1. If you complete the try-catch group and try to run the program at this point, you'll get a warning from log4j that an appender hasn't been setup so log4j hasn't been properly initialized. The problem is that the log4j.properties file isn't included in the classpath (it's in the same folder as your Eclipse build path, but Eclipse doesn't have it in its “runtime classpath”.
  1. To add the log4j.properties file to the Eclipse classpath, click on the Run menu and select the Run Configurations option. Select the Classpath tab. Left-click on the “User Entries” item and then select the Advanced … option. Click the Add External Folder option and then click OK button. Navigate to the folder where you unzipped the crawler4j-x.x zip file and select it.
  1. When you're done, the folder that you selected will be listed on the Classpath tab under User Entries.
  1. At this point you are pretty much set up to run crawler4j. I've always found it helpful to be able to refer to the source code to better understand what the objects that I'm using are actually doing. Fortunately the source code is available thru Git.
  1. Eclipse Kepler has the necessary perspective to get a clone of the crawler4j source code. Get a clone of the crawler4j master and save it to a folder on your local system.
  1. Right click on the crawler4j-x.x.jar and select the Build Path → Configure Build Path option.
  1. Select the Libraries tab and then expand the crawler4j-x.x.jar entry. Left-click on the Source Attachment option and then click on the Edit button.
  1. Navigate to the location of the crawler4j source files and select that folder. I had trouble getting the right folder selected so I zipped the source files, starting at the edu folder, and selected the zip file instead.
  1. When all is setup correctly, you can double-click on the individual class files in the crawler4j-x.x.jar file (in the Referenced Libraries) and the matching .java file is displayed.

**Ready to Test Drive**

At this point we're pretty much all set up and ready to test drive crawler4j. But before we take it out for a spin, a few words about how a crawler4j webcrawler is structured and who the pieces work.

On the home page for crawler4j, the Sample Usage section provides a lot of useful information that, honestly, didn't mean very much to me until I read through a lot of the source code and ran the Basic crawler example.

Every webcrawler has two main pieces: the “crawler” and the “crawl controller”. If some additional functionality is required, you can write the code and add it to your webcrawler.

The crawler has the patterns (file extensions) to match, such as png, mp3, zip, etc).

The shouldVisit(WebURL) method should return true when you want your webcrawler to visit the WebURL parameter. You are responsible for defining when to return true or false based on the  WebURL parameter (match or !match) any of the specified filter patterns.

The visit(Page) method extracts info from the retrieved web page. You can get information about the page, such as its URL, domain, path, parent page, text length, etc. These can be output to the console, as is done in the `BasicCrawler` or the info can be saved, as you desire.

The crawl controller is responsible for setting up the configuration of the webcrawler and creating an instance of a `CrawlController` that includes the appropriate configuration info. It also adds the “seeds” to the `CrawlController` object to tell it which web site(s) to visit. When everything is setup, the `CrawlController`.start() method is called to start the crawling process.

So we're ready to run our first webcrawler, the Basic crawler linked to the crawler4j home page. You can copy and paste from the git repository or you can use the example that was included in the git clone of crawler4j that you downloaded.

**Copy and Paste method**
  1. In the Example Codes section of the crawler4j home page, select the Basic crawler link. This will take you to the git repository for crawler4j source code. (See alt procedure at end of doc).
  1. Open `BasicCrawler`.java. Take note of the package.
  1. In Eclipse, create a new package that matches the package specified in `BasicCrawler`.java.
  1. In Eclipse, create a new Class named `BasicCrawler`. Make sure that its package is correct, but you don't really need anything in the file.
  1. Switch back to `BasicCrawler`.java source code. Select all the code and copy it to the clipboard.
  1. Switch back to Eclipse and paste all of the copied code into the new class.
  1. Repeat steps 2 through 6 for `BasicCrawlController`.java
  1. Fix any problems that Eclipse is complaining about.

**Use Downloaded Examples in Eclipse**
  1. Open Windows Explorer or similar visual file manager and navigate to the local clone of the crawler4j repository.
  1. Navigate to the src/test/java folder.
  1. Click and drag the edu folder and drop it on the src folder in your project in Eclipse.
  1. If the package named test shows errors, just delete it. It isn't needed to run the examples.
  1. Expand the basic package and double-click on both `BasicCrawlController` and `BasicCrawler`.

**Run the `BasicCrawler` example**
  1. OPTIONAL for first run: modify the code at the top of `BasicCrawlController` code to comment out the argument processing lines. In Eclipse, its easier to hard code in the crawl storage folder and number of crawlers (threads) than it is to muck around with setting up and modifying the parameters on the Arguments tab.
  1. OPTIONAL for first run: modify the controller seeds that are added at the bottom of the `BasicCrawlController` code. I commented out two of the seeds for this test run.
  1. Select Run → Run Configurations option and select your project and `BasicCrawlController` as the main class.  Click Apply button and then Run to start your first webcrawler.
  1. When it starts running, it will include info similar to the following:
```
Deleting content of: C:\asp\crawler4jStorage\frontier
 INFO [main] Crawler 1 started.
 INFO [main] Crawler 2 started.
```
  1. Then for each web site visited, it will generate (to console or other) info similar to:
```
Docid: 1
URL: http://www.ics.uci.edu/~lopes/
Domain: 'uci.edu'
Sub-domain: 'www.ics'
Path: '/~lopes/'
Parent page: null
Anchor text: null
Text length: 2442
Html length: 9987
Number of outgoing links: 34
Response headers:
	Date: Fri, 07 Mar 2014 07:40:21 GMT
	Server: Apache/2.2.15 (CentOS)
	Last-Modified: Tue, 07 Jan 2014 17:41:36 GMT
	ETag: "672c0db5-2703-4ef64e34f02c7"
	Accept-Ranges: bytes
	Content-Length: 9987
	Connection: close
	Content-Type: text/html; charset=UTF-8
	Set-Cookie: Coyote-2-80c30153=80c3019d:0; path=/
=============
```
  1. When the webcrawler finishes, it will display messages similar to the following:
```
 INFO [Thread-1] It looks like no thread is working, waiting for 10 seconds to make sure...
 INFO [Thread-1] No thread is working and no more URLs are in queue waiting for another 10 seconds to make sure...
 INFO [Thread-1] All of the crawlers are stopped. Finishing the process...
 INFO [Thread-1] Waiting for 10 seconds before final clean up...
```
At this point, you are ready to experiment more with the basic crawler or any of the other crawlers provided with crawler4j.

Dig in and have fun!