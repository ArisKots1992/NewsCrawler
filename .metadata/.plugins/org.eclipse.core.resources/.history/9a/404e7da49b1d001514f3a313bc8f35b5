package Crawler;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.io.UnsupportedEncodingException;
import java.util.ArrayList;
import java.util.Date;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import org.w3c.dom.Document;
import org.w3c.dom.Element;

public class Site {
	String SiteRSS = "";
	String SiteName = "";
	//String myPath = "C:\\Users\\Aris\\Desktop\\NewsParadise"; 
	String myPath = System.getProperty("user.dir"); 

	ArrayList<Article> AritcleList = new ArrayList<Article>();
	
	public Site(String SiteRSS,String SiteName){
		
		this.SiteRSS = SiteRSS;
		this.SiteName = SiteName;
		
	}
	public void addArticle(String Title,Date Publish_Date,String Link,String Description,String MainBody){
		AritcleList.add(new Article(Title,Publish_Date,Link,Description,MainBody));
	}
	public void print(){
		
		System.out.println("===================================================================================");
		System.out.println("=============>>>" +  SiteName);
		System.out.println("===================================================================================");
		System.out.println();
		
		for(int i=0; i<this.AritcleList.size(); i++)
		{	System.out.println("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");
			System.out.println("   ---> Article(" + i + ")");
			System.out.println("Date :" + this.AritcleList.get(i).Publish_Date);
			System.out.println("Publish_Date :" + this.AritcleList.get(i).Publish_Date);
			System.out.println("Link :" + this.AritcleList.get(i).Link);
			System.out.println("Description :" + this.AritcleList.get(i).Description);
			System.out.println("MainBody :" + this.AritcleList.get(i).MainBody);
		}
	}
	public void printToFile() throws FileNotFoundException, UnsupportedEncodingException{
		
		File file = new File(myPath);
		if (!file.exists()) {
			if (file.mkdir()) {
				System.out.println("Directory is created!");
			} else {
				System.out.println("Failed to create directory!");
			}
		}
		PrintWriter writer = new PrintWriter(myPath + "//" + SiteName + ".txt", "UTF-8");
		
		writer.println("===================================================================================");
		writer.println("=============>>>" +  SiteName);
		writer.println("===================================================================================");
		writer.println();
		
		for(int i=0; i<this.AritcleList.size(); i++)
		{	
			writer.println("   ---> Article(" + i + ")");
			writer.println("Date :" + this.AritcleList.get(i).Publish_Date);
			writer.println("Publish_Date :" + this.AritcleList.get(i).Publish_Date);
			writer.println("Link :" + this.AritcleList.get(i).Link);
			writer.println("Description :" + this.AritcleList.get(i).Description);
			writer.println("MainBody :" + this.AritcleList.get(i).MainBody);
			writer.println("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");
		}
		writer.close();

	}
	public void printToXML() throws  ParserConfigurationException, TransformerException{
		
		DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
		DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
 
		// root element
		Document doc = docBuilder.newDocument();
		Element rootElement = doc.createElement("site");
		rootElement.setAttribute("name", SiteName);
		doc.appendChild(rootElement);
		
		for(int i=0; i<this.AritcleList.size(); i++)
		{	
			// Article elements
			Element Article = doc.createElement("Article");
			rootElement.appendChild(Article);
			//set attribute to Article
			Article.setAttribute("id", String.valueOf(i));

			// Publish_date elements
			Element date = doc.createElement("date");
			if(this.AritcleList.get(i).Publish_Date == null)
				date.appendChild(doc.createTextNode(""));
			else
				date.appendChild(doc.createTextNode(String.valueOf(this.AritcleList.get(i).Publish_Date)));
			Article.appendChild(date);	
			
			// Title elements
			Element title = doc.createElement("title");
			title.appendChild(doc.createTextNode(this.AritcleList.get(i).Title));
			Article.appendChild(title);
			
			// Link elements
			Element Link = doc.createElement("Link");
			Link.appendChild(doc.createTextNode(this.AritcleList.get(i).Link));
			Article.appendChild(Link);	
			
			// Description elements
			Element Description = doc.createElement("Description");
			Description.appendChild(doc.createTextNode(this.AritcleList.get(i).Description));
			Article.appendChild(Description);	
			
			// MainBody elements
			Element MainBody = doc.createElement("MainBody");
		//	MainBody.appendChild(doc.createTextNode(this.AritcleList.get(i).MainBody))
			MainBody.appendChild(doc.createTextNode(this.AritcleList.get(i).MainBody.replaceAll("[^\\x00-\\x7F]", "")));
			Article.appendChild(MainBody);	
		}
		
		// write the content into xml file
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		DOMSource source = new DOMSource(doc);
		//StreamResult result = new StreamResult(new File(myPath + "\\" + SiteName + ".xml"));
		//Create folder
		File theDir = new File("filesXml");

		// if the directory does not exist, create it
		if (!theDir.exists()) {
		    boolean result = false;
		    try{
		        theDir.mkdir();
		        result = true;
		    } 
		    catch(SecurityException se){
		        //handle it
		    }        
		    if(result) {    
		        System.out.println("DIR created");  
		    }
		}
		
		StreamResult result = new StreamResult(new File(theDir.getPath() + "/" + SiteName + ".xml"));

		
		transformer.setOutputProperty(OutputKeys.INDENT, "yes");
		transformer.setOutputProperty(OutputKeys.METHOD, "xml");
		transformer.setOutputProperty("{http://xml.apache.org/xslt}indent-amount", "4");
		// Output to console for testing
		// StreamResult result = new StreamResult(System.out);
 
		transformer.transform(source, result);

	}
}
class Article {

	String Title = "";
	Date Publish_Date = null;
	String Link = "";
	String MainBody = "";
	String Description = "";
	
	public Article(String Title,Date Publish_Date,String Link,String Description,String MainBody){
		this.Title = Title;
		this.Publish_Date = Publish_Date;
		this.Link = Link;
		this.Description = Description;
		this.MainBody = MainBody;
	}
}