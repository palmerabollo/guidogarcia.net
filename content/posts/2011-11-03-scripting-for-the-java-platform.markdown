---
author: admin
date: 2011-11-03 07:44:57+00:00
draft: false
title: Scripting for the Java platform
type: post
url: /blog/2011/11/03/scripting-for-the-java-platform/
categories:
- Software
- Tecnología
tags:
- java
- javascript
- script
---


Despite many developers do not know it, since Java SE 6 it is really easy to integrate Java and some scripting languages through a standard Java Scripting API ([JSR-223](http://www.jcp.org/en/jsr/detail?id=223) implementation)







Currently, both AppleScript (2.2.1) and ECMAScript (1.6) script engines are supported by default, at least on my machine. In the following example I invoke a Javascript function from Java, using [Mozilla Rhino](http://www.mozilla.org/rhino/), the JavaScript implementation that ships with Java SE 6:





    
    
    import javax.script.Invocable;
    import javax.script.ScriptEngine;
    import javax.script.ScriptEngineManager;
    
    public class TestScript {
      public static void main(String[] args) throws Exception {
        ScriptEngineManager mgr = new ScriptEngineManager();
        ScriptEngine engine = mgr.getEngineByName("js");
        
        String script = "function sum(a, b) {" +
                        "   var result = a + b;" +
                        "   return result;" +
                        "}";
        engine.eval(script);
    
        Invocable invocableEngine = (Invocable) engine;
        Number result = 
                (Number) invocableEngine.invokeFunction("sum", 2, 3);
        System.out.println(result); // output = 5.0
      }
    }
    





### Reuse existing code






I find this "trick" useful in cases where you want to reuse existing scripting code. In the following example I use the great [datejs library](http://www.datejs.com/) to parse dates written in natural language:





    
    
    import java.io.InputStreamReader;
    import java.io.Reader;
    
    import javax.script.Invocable;
    import javax.script.ScriptEngine;
    import javax.script.ScriptEngineManager;
    import javax.script.ScriptException;
    
    public class DateParser {
        private final ScriptEngine engine;
        
        public DateParser() throws ScriptException {
            ScriptEngineManager mgr = new ScriptEngineManager();
            engine = mgr.getEngineByName("js");
            
            Reader reader = new InputStreamReader(
                    this.getClass().getResourceAsStream("/scripts/date.js"));
            engine.eval(reader);
            engine.eval(
            	"function parse(value) { " +
            	"    return Date.parse(value).toDateString();" +
            	"};"
            );
        }
    
        public String parse(String naturalLanguageDate)
                throws ScriptException, NoSuchMethodException {
            Invocable invocableEngine = (Invocable) engine;
            return (String) invocableEngine.invokeFunction("parse", naturalLanguageDate);
        }
    
        public static void main(String[] args) throws Exception {
            DateParser parser = new DateParser();
            
            // output = Thu Nov 03 2011
            System.out.println(parser.parse("next thursday")); 
    
            // output  = Wed Nov 09 2011
            System.out.println(parser.parse("tomorrow + 10 days"));
        }
    }
    






Well, that is all for the proof of concept I was looking for. If you want to read a more complete and deep article about this subject, I recommend [Java Scripting Programmer's Guide at Oracle](http://download.oracle.com/javase/6/docs/technotes/guides/scripting/programmer_guide/index.html).






### Problems found






So far, the most problematic point I have found is the conversion from the `Object` returned by `invokeFunction` to the appropriate Java types. It is not hard when it is a simple type such as a Number or a String, but it is not so easy when it comes to a more complex type (`Array`, `Date`). This is why the `parse` function in the second example returns a `String` instead of a `Date`.







In my first attempt I tried to create a neural network with [brain](http://harthur.github.com/brain/), a javascript supervised machine learning library. It would have been an interesting and even useful piece of code, but I failed miserably, probably because they use code not supported by ECMAScript 1.6.







A little benchmark would also be great, to determine the overhead introduced (if any) when you execute scripting code from Java, and compare it with a external/standalone script engine.

