package Crawler;

import java.net.URL;
import java.util.ArrayList;
import java.util.Date;
import java.util.Iterator;
import java.util.List;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

import com.sun.syndication.feed.synd.SyndEntry;
import com.sun.syndication.feed.synd.SyndFeed;
import com.sun.syndication.io.SyndFeedInput;
import com.sun.syndication.io.XmlReader;

import de.l3s.boilerpipe.extractors.ArticleExtractor;

public class Main {

	static ArrayList<Site> SiteList = new ArrayList<Site>();

	class MyThread extends Thread {

		private String Name, Rss;

		public MyThread(String Name, String Rss) {
			this.Name = Name;
			this.Rss = Rss;
		}

		public void run() {
			doSomeWork();
		}

		private void doSomeWork() {
			try {
				Site site = new Site(Rss, Name);

				// Crawl all RSS News Feed
				URL url = new URL(Rss);
				XmlReader reader = null;

				reader = new XmlReader(url);
				SyndFeed feed = new SyndFeedInput().build(reader);

				List entries = feed.getEntries();
				Iterator itEntries = entries.iterator();

				while (itEntries.hasNext()) {
					SyndEntry entry = (SyndEntry) itEntries.next();

					// GET title, date, Link , Description
					String TITLE = ((entry.getTitle() == null) ? "" : entry
							.getTitle());
					Date DATE = ((entry.getPublishedDate() == null) ? null
							: entry.getPublishedDate());
					String LINK = ((entry.getLink() == null) ? "" : entry
							.getLink());

					String desc = entry.getDescription().getValue();
					int x = desc.indexOf("<");

					String DESCRIPTION = ((x >= 0) ? desc.substring(0, x)
							: desc);
					
					String MAIN_BODY = "";
					
					// Get Main Body here
					if (LINK.length() != 0 && LINK.contains("http")) {
						URL MainBodyUrl = new URL(LINK);
						try {
							MAIN_BODY = ArticleExtractor.INSTANCE
									.getText(MainBodyUrl);
							// Our Object is ready now to be added to the List
							site.addArticle(TITLE, DATE, LINK, DESCRIPTION, MAIN_BODY);
						} catch (Exception e) {
							MAIN_BODY = "";
							//e.printStackTrace();
						}
					}
				}
				System.out.println(Name + " Done!");
				// OUR SITE FINISHED SO LETS ADD IT TO OUR SITE LIST
				synchronized (this) {
					SiteList.add(site);
				}

			} catch (Exception ex) {
				System.out.print(ex);
			}
		}
	}

	private void doWithCachedThreadPool() throws Exception {
		Global.setTrustAllCerts();
		// Here we store everything about the site
		ExecutorService executor = Executors.newCachedThreadPool();

		// for each site get its rss news and store them in Site class.
		for (String key : Global.RSS.keySet()) {
			String Name = key;
			String Rss = Global.RSS.get(key);
			// INSIDE THREAD//////

			MyThread thread = new MyThread(Name, Rss);
			executor.execute(thread);

		}
		executor.shutdown();
		executor.awaitTermination(Long.MAX_VALUE, TimeUnit.NANOSECONDS);
		
//		while (!executor.awaitTermination(3, TimeUnit.SECONDS)) {}
		
//		while (true) {
//			if (((ThreadPoolExecutor) executor).getActiveCount() == 0)
//				break;
//		}
		System.out.println("Well Done Lets Create the XML files!");
		for (int i = 0; i < SiteList.size(); i++) {
			//SiteList.get(i).print();
			SiteList.get(i).printToXML();
		}
	}

	public static void main(String[] args) throws Exception {

		Main threadPool = new Main();

		try {
			threadPool.doWithCachedThreadPool();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}

}
