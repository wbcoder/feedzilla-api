# Feedzilla API Specification #
**Here we describe in detail the "Application Programming Interface" (API) of Feedzilla. Programmers can use the Feedzilla API to make applications, websites, widgets, and other projects that interact with Feedzilla's data.**

## Getting Started ##
The following are the topics covered by this document.

### Topics ###
  * [Introduction](#Introduction.md)
    * [General](#General.md)
    * [Response Codes and Error Messages](#Response_Codes_and_Error_Messages.md)
      * [HTTP Status Codes](#HTTP_Status_Codes.md)
      * [Error Messages](#Error_Messages.md)
    * [A command line is all you need to use the Feedzilla API](#A_command_line_is_all_you_need_to_use_the_Feedzilla_API.md)
  * [REST API Documentation](#REST_API_Documentation.md)
    * [Resources](#Resources.md)
    * [Resource Methods](#Resource_Methods.md)
      * [Cultures Methods](#Cultures_Methods.md)
      * [Categories and Subcategories Methods](#Categories_and_Subcategories_Methods.md)
      * [Articles Methods](#Articles_Methods.md)
  * [Discussion](#Discussion.md)
  * [Terms of Service](#Terms_of_Service.md)

---

## Introduction ##
### General ###
The API attempts to conform to the design principles of [Representational State Transfer](http://en.wikipedia.org/wiki/Representational_State_Transfer) (REST).

The API presently supports the following data formats: XML, JSON, and the RSS and Atom syndication formats, with some methods only accepting a subset of these formats.

The following requests a JSON response format by using the "json" extension:

> `http://api.feedzilla.com/v1/cultures.json`

Parameter values should be converted to UTF-8 and URL encoded.

All Feedzilla APIs support an optional return format parameter. Note that json is the default response format. XML is also available. Some endpoints also support a simple txt format.

All Feedzilla APIs support jsonp which is the json format with a callback specified, such as:

> `.json&`**callback=callback\_method**

The application communicating with a resource does not need information concerning firewalls, gateways, proxies or other network devices between the application and the server holding the resource.

All dates will be treated as UTC and their formats must be YYYY-DD-MM.

All published date time objects are UTC based and conform to [RFC822.](http://asg.web.cmu.edu/rfc/rfc822.html)

### Response Codes and Error Messages ###
#### HTTP Status Codes ####
The Feedzilla API attempts to return appropriate HTTP status codes for every request.

  * **200 OK**: Success!
  * **304 Not Modified**: There was no new data to return.
  * **400 Bad Request**: The request was invalid.  An accompanying error message will explain why.
  * **403 Forbidden**: The request is understood, but it has been refused.  An accompanying error message will explain why.
  * **404 Not Found**: The URI requested is invalid or the resource requested, such as a category, does not exists.
  * **500 Internal Server Error**: Something is broken.
  * **502 Bad Gateway**: Feedzilla is down or being upgraded.
  * **503 Service Unavailable**: The Feedzilla servers are up, but overloaded with requests. Try again later.

#### Error Messages ####
When the API returns error messages, it does so in your requested format. For example, an error from an XML method might look like this:

```
    <error>
        <statuscode>400</statuscode>
        <statusdescription>The specified category was not found</statusdescription>
    </error>
```

#### A command line is all you need to use the Feedzilla API ####
If your system has [curl](http://curl.haxx.se/) (and it should!), you’ve already got a great way to poke around the Feedzilla API.

#### Entity-encoded HTML in Title, Description (Summary)  and Content ####
The Title, Description (Summary) and Content fields might contain entity-encoded HTML. To be able to display these fields as HTML they must be entity-decoded.

---

## REST API ##
### Resources ###
The following are definitions of the resources that are exposed by the API:

| **Resource Name** | **Description** |
|:------------------|:----------------|
| Culture           | Describes a culture resource. (en\_us for instance) |
| Category          | Describes a category of source feeds |
| Subcategory       | Describes a subcategory of source feeds |
| Articles          | Describes a set articles |

---

### Resource Methods ###
The API supports the following methods to send and receive Feedzilla data.

**Base URI:** http://api.feedzilla.com
### Cultures Methods ###
### /v1/cultures._format_ ###
**Description:** Returns a list of supported cultures.

**Formats:** xml, json

**HTTP Method(s):** GET

**Parameters:**
  * None

**Examples:**

  * XML format: "curl http://api.feedzilla.com/v1/cultures.xml"

```

	<culture>
		<a:culture_code>pt-BR_br</a:culture_code>
		<a:display_culture_name>Brasil</a:display_culture_name>
		<a:english_culture_name>Brazil</a:english_culture_name>
	</culture>

```

  * JSON format: "curl http://api.feedzilla.com/v1/cultures.json"

```
	{
		"culture_code":"au",
		"display_culture_name":"Australlia",
		"english_culture_name":"Australia"
	},

```


---

### Categories and Subcategories Methods ###
### /v1/categories._format_ ###
**Description**: Returns a list of supported categories.

**Formats:** xml, json

**HTTP Method(s):** GET

**Parameters:**
  * culture\_code - Optional. Restricts the results to the given culture. for instance japan is jp. Default is en\_us.
  * order - Optional. The sort order of the list to be returned. Can be one of the following:
    * popular - The list will be ordered by item popularity (most popular is first).
    * none - No special order. This is the default value.

**Examples:**

  * XML format: "curl http://api.feedzilla.com/v1/categories.xml"

```

	<category>
		<a:category_id>19</a:category_id>
		<a:display_category_name>World News</a:display_category_name>
		<a:english_category_name>World News</a:english_category_name>
		<a:url_category_name>world-news</a:url_category_name>
	</category>

```

  * JSON format: "curl http://api.feedzilla.com/v1/categories.json"

```

	{
		"category_id":8,
		"display_category_name":"Science",
		"english_category_name":"Science",
		"url_category_name":"science"
	},

```


---

### /v1/subcategories._format_ ###
**Description:** Returns a list of supported subcategories hierarchy.

**Formats:** xml, json

**HTTP Method(s):** GET

**Parameters:**
  * culture\_code - Optional. Restricts the results to the given culture. for instance japan is jp. Default is en\_us.
  * order - Optional. The sort order of the list to be returned. Can be one of the following:
    * popular - The list will be ordered by item popularity (most popular is first).
    * none - No special order. This is the default value.

**Examples:**

  * XML format: "curl http://api.feedzilla.com/v1/subcategories.xml"

```

	<subcategory>
		<a:category_id>2</a:category_id>
		<a:display_subcategory_name>Accounting</a:display_subcategory_name>
		<a:english_subcategory_name>Accounting</a:english_subcategory_name>
		<a:subcategory_id>2</a:subcategory_id>
		<a:url_subcategory_name>accounting</a:url_subcategory_name>
	</subcategory>

```

  * JSON format: "curl http://api.feedzilla.com/v1/subcategories.json"

```

	{
		"category_id":11,
		"display_subcategory_name":"Addiction",
		"english_subcategory_name":"Addiction",
		"subcategory_id":440,"url_subcategory_name":"addiction"
	},

```


---

### /v1/categories/_{category\_id}_/subcategories._format_ ###
**Description:** Returns a list of supported subcategories for the category given by category\_id.

**Formats:** xml, json

**HTTP Method(s):** GET

**Parameters**:
  * order - Optional. The sort order of the list to be returned. Can be one of the following:
    * popular - The list will be ordered by item popularity (most popular is first).
    * none - No special order. This is the default value.

**Examples:**

  * XML format: "curl http://api.feedzilla.com/v1/categories/26/subcategories.xml"

```

	<subcategory>
		<a:category_id>26</a:category_id>
		<a:display_subcategory_name>Internet</a:display_subcategory_name>
		<a:english_subcategory_name>Internet</a:english_subcategory_name>
		<a:subcategory_id>1308</a:subcategory_id>
		<a:url_subcategory_name>internet</a:url_subcategory_name>
	</subcategory>

```

  * JSON format: "curl http://api.feedzilla.com/v1/categories/13/subcategories.json"

```

    {
	"category_id":13,
	"display_subcategory_name":"Performing Arts",
	"english_subcategory_name":"Performing Arts",
	"subcategory_id":592,
	"url_subcategory_name":"performing-arts"
    },

```


---

### Articles Methods ###
#### Usage Notes: ####
  * Search query must be [URL Encoded](http://en.wikipedia.org/wiki/URL_encoding).
  * Search query is limited to 100 encoded characters.
**Sample JSON Result for all articles methods:**
```
    {
       "articles":[
          {
             "url":"http://proxy.feedzilla.com/r/35423",
             "title":"Feedzilla: News edition has changed! Please read more!",
             "summary":"Please read the article when possible, we are likely to get used to it!",
             "publish_date":"Thu, 01 Jul 2010 14:14:38 +0000",
             "author":"Feedzilla Team",
             "source":"CNN"
          },
          {
             "url":"http://proxy.feedzilla.com/r/4123",
             "title":"Feedzilla: News edition has changed! Please read more!",
             "summary":"Another sample article. Please read the article when possible, we are likely to get used to it!",
             "publish_date":"Thu, 01 Jul 2010 14:14:38 +0000",
             "author":"Feedzilla Team",
             "source":"Feedzilla"
          }
       ],
       "syndication_url":"http://feeds.feedzilla.com/en_us/news/business.rss"-
    }
```

---

### /v1/categories/_{category\_id}_/articles._format_ ###
**Description:** Returns most recent articles for the category given by category\_id.

**Formats:** json, rss, atom

**HTTP Method(s):** GET

**Parameters:**
  * count -  Optional.  Specifies the number of articles to retrieve. May not be greater than 100. (Note that the number of articles returned may be smaller than the requested count). **Default is 20.**
  * since -  Optional.  Returns articles that was published since the given date. Date should be formatted as YYYY-MM-DD
  * order - Optional. The sort order of the list to be returned. Can be one of the following:
    * relevance - The list will be ordered by article relevancy (most relevant is first). **This is the default value.**
    * date - The list will be ordered by article publication date (most recent is first).
  * client\_source - Optional. A string representing the client that uses this request.
  * title\_only - Optional. A value (1 - true, 0 - false) indicating whether to fetch article title only (no summary or content). **Default is 0.**

**Examples:**

  * JSON format: "curl http://api.feedzilla.com/v1/categories/26/articles.json"

```
{
 "articles":[
 
	{
		"publish_date":"Tue, 05 Oct 2010 14:11:00 +0100",
		"source":"tiscali.co.uk",
		"source_url":"http:\/\/www.talktalk.co.uk\/rss\/entertainment.xml",
		"summary":"Will Ferrell and Mark Wahlberg clung on to top spot at the box office...",
		"title":"\"The Other Guys\" stays top of UK box office (tiscali.co.uk)",
		"url":"http:\/\/news.feedzilla.com\/en_us\/stories\/top-news\/15587113?client_source=api&format=json"
	},]
}

```

  * RSS format: "curl http://api.feedzilla.com/v1/categories/13/articles.rss?title_only=1"

```
?<?xml version="1.0" encoding="utf-8"?><?xml-stylesheet type="text/xsl" href="/styles/rss20.xsl"?>	
```
```
	<item>
		<guid isPermaLink="false">feedzilla.com15558373</guid>
		<link>http://news.feedzilla.com/en_us/stories/art/15558373?client_source=api&amp;format=rss</link>
		<title>Bokeh (About)</title>
		<description>&lt;div&gt;Share With Friends: &lt;ahref="http://news.feedzilla.com/en_us/share/art/15558373?............t;Feedzilla.com&lt;/a&gt;&lt;/div&gt;</description>
		<source url="http://www.about.com/6/o/m/photography_p2.xml">About</source>
		<pubDate>Tue, 05 Oct 2010 11:55:00 +0100</pubDate>
	</item>

```

  * ATOM format: "curl http://api.feedzilla.com/v1/categories/27/articles.atom?count=10"

```
?<?xml version="1.0" encoding="utf-8"?><?xml-stylesheet type="text/xsl" href="/styles/atom10.xsl"?>
```
```
	<entry>
		<id>feedzilla.com15596191</id>
		<title type="html">Drew Brees can't say enough about RB Ladell Betts (The Canadian Press) (Yahoo!)</title>
		<summary type="html">METAIRIE, La. - New Orleans quarterback Drew Brees says you can't put a number on the value of Ladell Betts....</summary>
		<published>2010-10-05T13:45:00+01:00</published>
		<updated>2010-10-05T13:45:00+01:00</updated>
		<author><name /></author>
		<link rel="alternate" href="http://news.feedzilla.com/en_us/stories/sports/15596191?count=10&amp;client_source=api&amp;format=atom" />
		<rights type="text"></rights>
		<source>
			<title type="text">Yahoo!</title>
			<link rel="self" href="http://sports.yahoo.com/nfl/teams/was/rss.xml" />
		</source>
	</entry>

```

---

### /v1/categories/_{category\_id}_/articles/search._format_ ###
**Description:** Returns most recent articles for the category given by category\_id and match a specified query.

**Formats:** json, rss, atom

**HTTP Method(s):** GET

**Parameters:**
  * q - Required .  The text to search for in articles.
  * count -  Optional.  Specifies the number of articles to retrieve. May not be greater than 100. (Note the the number of articles returned may be smaller than the requested count). Default is 20.
  * since -  Optional.  Returns articles that was published since the given date. Date should be formatted as YYYY-MM-DD
  * order - Optional. The sort order of the list to be returned. Can be one of the following:
    * relevance - The list will be ordered by article relevancy (most relevant is first). **This is the default value.**
    * date - The list will be ordered by article publication date (most recent is first).
  * client\_source - Optional. A string representing the client that uses this request.
  * title\_only - Optional. A value (1 - true, 0 - false) indicating whether to fetch article title only (no summary or content). **Default is 0.**

**Examples:**

  * JSON format: "curl http://api.feedzilla.com/v1/categories/26/articles/search.json?q=Michael"

```
{
 "articles":[
 
	{
	
		"author":"By WILBORN HAMPTON",
		"publish_date":"Tue, 05 Oct 2010 05:30:00 +0100",
		"source":"International Herald Tribune",
		"source_url":"http:\/\/feeds.nytimes.com\/nyt\/rss\/Arts",
		"summary":"“The Drunkard”: Howard Thoresen, left, and Michael Hardart in this 1800s work at the Metropolitan Playhouse.",
		"title":"Theater Review | 'The Drunkard': Saved, Before Alcoholics Anonymous (International Herald Tribune)",
		"url":"http:\/\/news.feedzilla.com\/en_us\/stories\/top-news\/15461573?q=michael&client_source=api&format=json"
	
	},]
 
}
```

  * RSS format: "curl http://api.feedzilla.com/v1/categories/13/articles/search.rss?q=Michael&title_only=1"

```
<rss version="2.0">
```
```
	<item>
		<guid isPermaLink="false">feedzilla.com15558351</guid>
		<link>http://news.feedzilla.com/en_us/stories/art/15558351?q=painting&amp;client_source=api&amp;format=rss</link>
		<title>Dancing a Painting (About)</title>
		<description>&lt;div&gt;Share With Friends: &lt;ahref="http://news.feedzil.....com/'&gt;Feedzilla.com&lt;/a&gt;&lt;/div&gt;</description>
		<source url="http://www.about.com/6/o/m/painting_t2.xml">About</source>
		<pubDate>Tue, 05 Oct 2010 11:08:00 +0100</pubDate>
	</item>

```

  * ATOM format: "curl http://api.feedzilla.com/v1/categories/27/articles/search.atom?q=Michael&count=10"

```
<feed xmlns="http://www.w3.org/2005/Atom">
```
```
	<entry>
		<id>feedzilla.com15549562</id>
		<title type="html">DRAYS BAY :: ALDS Link and Analysis: Michael Young, Cliff Lee, Ben Zobrist, and Jonah Keri (Sports Blog)</title>
		<summary type="html">&lt;div&gt;Share W................................;Feedzilla.com&lt;/a&gt;&lt;/div&gt;</summary>
		<published>2010-10-05T12:01:00+01:00</published>
		<updated>2010-10-05T12:01:00+01:00</updated>
		<author><name/></author>
		<link rel="alternate" href="http://news.feedzilla.com/en_us/stories/sports/15549562?count=10&amp;q=michael&amp;client_source=api&amp;format=atom"/>
		<rights type="text"/>
		<source>
			<title type="text">Sports Blog</title>
			<link rel="self" href="http://rss.sportsblogs.org/rss/mlb.xml"/>
		</source>
	</entry>

```


---

### /v1/categories/_{category\_id}_/subcategories/_{subcategory\_id}_/articles._format_ ###
**Description:** Returns most recent articles for the category specified by category\_id and subcategory specified by subcategory\_id.

**Formats:** json, rss, atom

**HTTP Method(s):** GET

**Parameters:**
  * count -  Optional.  Specifies the number of articles to retrieve. May not be greater than 100. (Note the the number of articles returned may be smaller than the requested count). Default is 20.
  * since -  Optional.  Returns articles that was published since the given date. Date should be formatted as YYYY-MM-DD
  * order - Optional. The sort order of the list to be returned. Can be one of the following:
    * relevance - The list will be ordered by article relevancy (most relevant is first). **This is the default value.**
    * date - The list will be ordered by article publication date (most recent is first).
  * client\_source - Optional. A string representing the client that uses this request.
  * title\_only - Optional. A value (1 - true, 0 - false) indicating whether to fetch article title only (no summary or content). **Default is 0.**

**Examples:**

  * JSON format: "curl http://api.feedzilla.com/v1/categories/26/subcategories/1303/articles.json"

```
{
 "articles":[
 
	{		
	
		"author":"Paul Chi",
		"publish_date":"Tue, 05 Oct 2010 14:30:00 +0100",
		"source":"People","source_url":"http:\/\/feeds.people.com\/people\/headlines",
		"summary":"Still, he feels comfortable with himself, and partly credits sister Maggie",
		"title":"Jake Gyllenhaal Dreads Turning 30 - Sort Of (People)",
		"url":"http:\/\/news.feedzilla.com\/en_us\/stories\/top-news\/celebrities\/15616550?client_source=api&format=json"
	
	},]

}
```

  * RSS format: "curl http://api.feedzilla.com/v1/categories/13/subcategories/589/articles.rss?title_only=1"

```
<rss version="2.0">
```
```
	<item>
		<guid isPermaLink="false">feedzilla.com14266379</guid>
		<link>http://news.feedzilla.com/en_us/stories/art/dance/14266379?client_source=api&amp;format=rss</link>
		<title>A New Graeme Murphy Dance Company (Arts Journal)</title>
		<description>&lt;div&gt;Share With Friends: &lt;ahref="http://news.feedzilla.com/en_us/share/art/dance/14266379......m/'&gt;Feedzilla.com&lt;/a&gt;&lt;/div&gt;</description>
		<source url="http://www.artsjournal.com/artsjournal1/dance.xml">Arts Journal</source>
		<pubDate>Fri, 01 Oct 2010 06:33:00 +0100</pubDate>
	</item>

```

  * ATOM format: "curl http://api.feedzilla.com/v1/categories/27/subcategories/1370/articles.atom?count=10"

```
?<?xml version="1.0" encoding="utf-8"?> <?xml-stylesheet type="text/xsl" href="/styles/atom10.xsl"?>
```
```

	<entry>
		<id>feedzilla.com15621161</id>
		<title type="html">Knicks focus on defense thanks to Dan D'Antoni (NY Daily News - Knicks Knation)</title>
		<summary type="html">PARIS - Dan D'Antoni's voice is being heard more and more in training camp and it's usually when the Knicks are working on def........</summary>
		<published>2010-10-05T15:43:00+01:00</published>
		<updated>2010-10-05T15:43:00+01:00</updated>
		<author><name /></author>	
		<link rel="alternate" href="http://news.feedzilla.com/en_us/stories/sports/basketball-nba/15621161?count=10&amp;client_source=api&amp;format=atom" />
		<rights type="text"></rights>
		<source>
			<title type="text">NY Daily News - Knicks Knation</title>
			<link rel="self" href="http://www.nydailynews.com/blogs/knicks/rss.xml" />
		</source>
	</entry>

```


---

### /v1/categories/_{category\_id}_/subcategories/_{subcategory\_id}_/articles/search._format_ ###

**Description:** Returns most recent articles for the category specified by category\_id and subcategory specified by subcategory\_id and match a specified query.

**Formats:** json, rss, atom

**HTTP Method(s):** GET

**Parameters:**
  * q - Required .  The text to search for in articles.
  * count -  Optional.  Specifies the number of articles to retrieve. May not be greater than 100. (Note the the number of articles returned may be smaller than the requested count). Default is 20.
  * since -  Optional.  Returns articles that was published since the given date. Date should be formatted as YYYY-MM-DD
  * order - Optional. The sort order of the list to be returned. Can be one of the following:
    * relevance - The list will be ordered by article relevancy (most relevant is first). **This is the default value.**
    * date - The list will be ordered by article publication date (most recent is first).
  * client\_source - Optional. A string representing the client that uses this request.
  * title\_only - Optional. A value (1 - true, 0 - false) indicating whether to fetch article title only (no summary or content). **Default is 0.**

**Examples:**

  * JSON format: curl "http://api.feedzilla.com/v1/categories/26/subcategories/1303/articles/search.json?q=Michael"

```
{
 "articles":[
 
	{		
	
		"author":"Tim Nudd and Michele Stueven",
		"publish_date":"Tue, 05 Oct 2010 15:00:00 +0100",
		"source":"People","source_url":"http:\/\/feeds.people.com\/people\/headlines",
		"summary":"The comedian and her Dancing With the Stars partner Louis van Amstel talk about being tormented as kids...",
		"title":"Margaret Cho: 'I Didn't Want to Live' When I Was Bullied (People)",
		"url":"http:\/\/news.feedzilla.com\/en_us\/stories\/top-news\/celebrities\/15616556?q=michael&client_source=api&format=json"
	
	},]

}
```

  * RSS format: "curl http://api.feedzilla.com/v1/categories/27/subcategories/1370/articles/search.rss?q=Michael&title_only=1"

```
<rss version="2.0">
```
```
	<item>
		<guid isPermaLink="false">feedzilla.com15620501</guid>
		<link>http://news.feedzilla.com/en_us/stories/sports/basketball-nba/15620501?q=michael&amp;client_source=api&amp;format=rss</link>
		<title>Turiaf excited about Knicks new roster (NBA)</title>
		<description>&lt;div&gt;Share With Friends: &lt;a href="http://news.feedzilla.com/en_.......;Feedzilla.com&lt;/a&gt;&lt;/div&gt;</description>
		<source url="http://www.nba.com/rss/nba_rss.xml">NBA</source>
		<pubDate>Tue, 05 Oct 2010 15:27:00 +0100</pubDate>
	</item>
	

```

  * ATOM format: "curl http://api.feedzilla.com/v1/categories/13/subcategories/589/articles/search.atom?q=Michael&count=10"

```
	<entry>
		<id>feedzilla.com14987817</id>
		<title type="html">Washington Ballet Ditches Live Music For 2010/11 (Arts Journal)</title>
		<summary type="html">The company's budget is $8 million, down from $8.3 million last year and $8.5 million in 2008........</summary>
		<published>2010-10-03T16:52:00+01:00</published>
		<updated>2010-10-03T16:52:00+01:00</updated>
		<author><name/></author>
		<link rel="alternate" href="http://news.feedzilla.com/en_us/stories/art/dance/14987817?count=10&amp;q=painting&amp;client_source=api&amp;format=atom"/>
		<rights type="text"/>
		<source>
			<title type="text">Arts Journal</title>
			<link rel="self" href="http://www.artsjournal.com/artsjournal1/dance.xml"/>
		</source>
	</entry>

```


---

### /v1/articles/search.format ###
**Description:** Returns the most recent articles that match a specified query.

**Formats:** json, rss, atom

**HTTP Method(s):** GET

**Parameters:**
  * culture\_code - Optional. Restricts the results to the given culture. **Default is en\_us.**
  * q - Required .  The text to search for in articles.
  * count -  Optional.  Specifies the number of articles to retrieve. May not be greater than 100. (Note the the number of articles returned may be smaller than the requested count). Default is 20.
  * since -  Optional.  Returns articles that was published since the given date. Date should be formatted as YYYY-MM-DD
  * order - Optional. The sort order of the list to be returned. Can be one of the following:
    * relevance - The list will be ordered by article relevancy (most relevant is first). **This is the default value.**
    * date - The list will be ordered by article publication date (most recent is first).
  * client\_source - Optional. A string representing the client that uses this request.
  * title\_only - Optional. A value (1 - true, 0 - false) indicating whether to fetch article title only (no summary or content). **Default is 0.**

**Examples:**

  * JSON formatl: "curl http://api.feedzilla.com/v1/articles/search.json?q=Michael"

```
{
 "articles":[
 
	{		

		"author":"Michael Maloney",
		"publish_date":"Tue, 05 Oct 2010 11:30:00 +0100",
		"source":"television",
		"source_url":"http:\/\/www.tvsquad.com\/rss.xml",
		"summary":"<p>Filed under: <ahref=\"http:\/\/www.tvsquad.com\/category\/features\/\" ..... ",
		"title":"RACHEL MCADAMS DATING MICHAEL SHEEN? (Showbiz Spy)",
		"url":"http:\/\/news.feedzilla.com\/en_us\/stories\/15583098?q=michael&client_source=api&format=json"

	
	},]

}
```

  * RSS format: "curl http://api.feedzilla.com/v1/articles/search.rss?q=Michael&title_only=1"

```
<rss version="2.0">
```
```
	<item>
		<guid isPermaLink="false">feedzilla.com15622757</guid>
		<link>http://news.feedzilla.com/en_us/stories/15622757?q=michael&amp;client_source=api&amp;format=rss</link>
		<title>September 2010 Top Ten: Our Most Popular Posts (Practical Ecommerce)</title>
		<description>&lt;div&gt;Share With Friends: &lt;a href="http://news.feedzilla.com/en_....Feedzilla.com&lt;/a&gt;&lt;/div&gt;</description>
		<source url="http://www.practicalecommerce.com/articles.rss">Practical Ecommerce</source>
		<pubDate>Tue, 05 Oct 2010 15:47:00 +0100</pubDate>
	</item>



```

  * ATOM format: "curl http://api.feedzilla.com/v1/articles/search.atom?q=Michael&count=10"

```
<?xml version="1.0" encoding="utf-8"?><?xml-stylesheet type="text/xsl" href="/styles/atom10.xsl"?>
```
```
	<entry>
		<id>feedzilla.com15595807</id>
		<title type="html">NBA 2K11 Ships (Gameinfowire.com)</title>
		<summary type="html">Relive Michael Jordan’s greatest moments with the all-new Jordan Challenge and more in NBA 2....../a&gt;&lt;/div&gt;</summary>
		<published>2010-10-05T14:31:00+01:00</published>
		<updated>2010-10-05T14:31:00+01:00</updated>
		<author><name /></author>
		<link rel="alternate" href="http://news.feedzilla.com/en_us/stories/15595807?count=10&amp;q=michael&amp;client_source=api&amp;format=atom" />	
		<rights type="text"></rights>
		<source>
			<title type="text">Gameinfowire.com</title>
			<link rel="self" href="http://www.gameinfowire.com/news.xml" />
		</source>
	</entry>
	

```


---

## Discussion ##
To report an error with a request or response please submit an issue:

> http://code.google.com/p/feedzilla-api/issues/list

We invite you to visit our Google Group to discuss technical issues related to using the API:
> http://groups.google.com/a/feedzilla.com/group/feedzilla-api

---

## Terms of Service ##
You can read the terms of service here:

> http://api.feedzilla.com/tos