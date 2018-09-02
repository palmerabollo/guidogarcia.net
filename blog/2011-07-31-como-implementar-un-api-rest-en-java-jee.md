---
author: admin
date: 2011-07-31 22:42:53+00:00
draft: false
title: Cómo implementar un API REST en Java/JEE
type: post
url: /blog/2011/07/31/como-implementar-un-api-rest-en-java-jee/
categories:
- Software
tags:
- java
- jaxrs
- jee
- rest
- spring
---

Me pidieron consejo hace unos días sobre este tema.  Personalmente, a día de hoy sólo recomiendo estas opciones, muy parecidas y que se basan en anotar las clases Java:






	  * El especificación **[JAX-RS](http://en.wikipedia.org/wiki/Java_API_for_RESTful_Web_Services)**, incluida de serie en Java EE 6. Hay varias implementaciones, entre otras [Jersey](http://jersey.java.net/), [Restlet](http://www.restlet.org/) o [Resteasy]( http://www.jboss.org/resteasy). Esta última es la que utiliza JBoss y que he utilizado sin problemas también con JBoss 5.1.





    
    
    @Path("/product/{id}")
    public class ProductResource {
        @GET
        public String get(@PathParam("id") String id) {
            return ...;
        }
    }
    








	  * **[Spring MVC](http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/mvc.html)**. Imagino que acabará convergiendo con JAX-RS ya que no tiene demasiado sentido mantener sus propias anotaciones. Yo he sido bastante fan de Spring, al que pronto dedicaré una serie de artículos completos. Fui reacio un tiempo por la documentación oficial, excesivamente dura para un novato, pero siempre han ido un paso por delante, son más ágiles que las JSR, que en ocasiones acaban pariendo clones de Spring (por ejemplo el [JSR 330](www.jcp.org/en/jsr/detail?id=330) para la inyección de dependencias).


    
    
    @Controller
    public class ProductResource {
        @RequestMapping(value="/product/{id}", method=RequestMethod.GET)
        public Product get(@PathVariable int id) {
            return ...;
        }
    }








	  * Para proyectos muy simples en los que no se quiera incluir ninguna dependencia, la esperada especificación de **Servlets 3.0**/[JSR 315](http://jcp.org/en/jsr/detail?id=315) (también de serie en JEE6 e implementada en Tomcat 7, Jetty 7, Glassfish 3, etc) incluye la anotación @WebServlet.  No la he utilizado nunca para estos fines, pero con algo más de esfuerzo de desarrollo en el procesamiento de las peticiones, podría valer.


    
    
    @WebServlet(urlPatterns={"/product/*"}
    public class ProductResource extends HttpServlet {
        public void service(ServletRequest req, ServletResponse res)
                throws IOException, ServletException {
            // Esta parte sería más fea
            // Parsear la URL para extraer parámetros...
        }
    }





## Mi consejo


Es difícil decantarse porque las diferencias acaban siendo sutiles. Dejo algunas consideraciones que se podrían tener en cuenta:






	  * Spring mola. Si ya utilizáis o conocéis Spring, usad Spring MVC, corre en un simple Tomcat sin más. Podréis aprovechar otras de sus deliciosas ventajas (inyección de dependencias, validaciones, etc).
	  * Si ya usáis un servidor de aplicaciones JEE6 (p.e. JBoss) usad JAX-RS. Si estáis usando Netbeans también es un punto a favor de JAX-RS porque viene bien integrado.
	  * Si no queréis utilizar un servidor de aplicaciones JEE6, usad Spring. Si queréis hacer algo simple (p.e. un piloto o una aplicación que va a recibir poca carga), utilizad Restlet que tiene una [extensión para JAX-RS](http://wiki.restlet.org/docs_2.1/13-restlet/28-restlet/57-restlet.html)  y corre como una aplicación Java, sin necesidad de ningún tipo de servidor web o de aplicaciones.



Espero vuestros comentarios, aunque sea para contar lo fácil que sería todo esto con Ruby on Rails.








  




_Disclaimer: Todos los extractos de código del artículo están escritos sin probar._

