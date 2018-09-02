---
author: admin
date: 2012-03-02 20:10:07+00:00
draft: false
title: How to develop a Java REST client in 3 minutes
type: post
url: /blog/2012/03/02/how-to-develop-java-rest-client/
categories:
- Software
- Tecnología
tags:
- cxf
- digg
- java
- jax-rs
- jboss
- jersey
- json
- resteasy
---

Some time ago I wrote a post about [how to implement a REST API with Java](http://guidogarcia.net/blog/2011/07/31/como-implementar-un-api-rest-en-java-jee/) (spanish). Today I am going to write about how to consume a REST API as a client. More specifically, I am going to use [Digg API](http://developers.digg.com/documentation) (probably not the best REST API out there) to search stories by a given keyword.



### Show me the code



As it is a third party REST API, I am going to use the [client framework provided by RESTEasy](http://docs.jboss.org/resteasy/docs/2.3.1.GA/userguide/html/RESTEasy_Client_Framework.html), that I find extremely easy to use.

You only have to add one dependency to your pom.xml:


    
    
      <dependency>
          <groupId>org.jboss.resteasy</groupId>
          <artifactId>resteasy-jaxrs</artifactId>
          <version>2.3.1.GA</version>
      </dependency>
    



And you are now ready to start coding. I think the Java code is self explanatory, so here it goes:


    
    
    import org.jboss.resteasy.client.ClientRequest;
    import org.jboss.resteasy.client.ClientResponse;
    
    public class DiggClient {
        private static final String DIGG_SEARCH_ENDPOINT =
                "http://services.digg.com/{version}/search.search";
    
        public static void main(String[] args) throws Exception {
            ClientRequest req = new ClientRequest(DIGG_SEARCH_ENDPOINT);
            req
                .pathParameter("version", "2.0")
                .queryParameter("query", "health");
    
            ClientResponse<String> res = req.get(String.class);
            System.out.println(res.getEntity());
        }
    }
    



**Update:** As pointed out by Andy MacKinlay in the comments, ClientRequest is deprecated in RESTEasy 3.x.



### But I do not want to parse a String...



You might have noticed that the response is converted to a String. In most cases an API will return a XML or a JSON representation of the data as response to the request. In these cases it is great to automatically convert the response into the objects in your Java data model.

For example, these are the domain classes representing (partially) the Digg search results:


    
    
    class SearchResult {
        private int total;
        private Story[] stories;
        // ... getters/setters
    }
    
    class Story {
        private String id;
        private String title;
        // ... getters/setters
    }
    



Digg results are offered as JSON, so you only need to add a Maven dependency to include the providers RESTEasy uses to handle JSON:


    
    
      <dependency>
          <groupId>org.jboss.resteasy</groupId>
          <artifactId>resteasy-jackson-provider</artifactId>
          <version>2.3.1.GA</version>
      </dependency>
    



The resulting Java code remains almost the same:


    
    
    import org.jboss.resteasy.client.ClientRequest;
    import org.jboss.resteasy.client.ClientResponse;
    
    public class DiggClient {
        private static final String DIGG_SEARCH_ENDPOINT =
                "http://services.digg.com/{version}/search.search";
    
        public static void main(String[] args) throws Exception {
            ClientRequest req = new ClientRequest(DIGG_SEARCH_ENDPOINT);
            req
                .pathParameter("version", "2.0")
                .queryParameter("query", "health");
    
            ClientResponse<SearchResult> res = req.get(SearchResult.class);
            SearchResult result = res.getEntity();
            System.out.println(result.getTotal() + " matching stories found");
        }
    }
    



Note: you need to annotate your domain Java classes as `@JsonIgnoreProperties(ignoreUnknown=true)` if the server returns any field that is not present in your domain Java classes.



### What if you are using your own API?



If you already have developed your JAX-RS resources, I would suggest to use a proxy-based approach. Take a look at the [JAX-RS client factory in Apache CXF](http://cxf.apache.org/docs/jax-rs-client-api.html#JAX-RSClientAPI-ProxybasedAPI), or how to [share interfaces between client and server with RESTEasy](http://docs.jboss.org/resteasy/docs/2.3.1.GA/userguide/html/RESTEasy_Client_Framework.html#Sharing_interfaces). This way you reuse more code and makes your code maintenance easier.

Comments about other alternatives ([Jersey](http://jersey.java.net/), etc) are welcome.
