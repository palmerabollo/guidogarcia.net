---
author: admin
date: 2012-08-26 17:42:41+00:00
draft: false
title: Analysis of variance (ANOVA) applied to fraud detection
type: post
url: /blog/2012/08/26/analysis-of-variance-anova-applied-to-fraud-detection/
categories:
- Software
- Tecnología
tags:
- anova
- apache
- fraud
- gather
- java
- math
- significance
- statistics
- survey
- variance
---

Fraud detection is a topic applicable to many sectors (financial, insurance, etc). The method explained in this post is applied in the market research field by [Gather Precision](http://precision.gatherestudios.es/) (a great market research tool developed by Gather Estudios, BTW), as an early signal to detect frauds in opinion polls.

Imagine you have four field workers (Peter, John, Mary, Ann) taking surveys on the street. They spend different times gathering the data, and we want to discover if there are significant differences on the average levels. That would mean that at least one of them is taking too few or too much time completing the surveys.


    
    
    Field worker = { Times in seconds for each survey he completes }
    Peter = { 150, 200, 180, 230, 220, 250, 230, 300 }
    John  = { 200, 240, 220, 250, 210, 190, 240 }
    Mary  = { 100, 130, 150, 180, 140, 200, 110, 120 }
    Ann   = { 200, 230, 150, 220, 210 }



This is one case where ANOVA comes to the rescue. According to wikipedia, in its simplest form, "ANOVA provides a statistical test of whether or not the means of several groups are all equal", and that is exactly what we are looking for.

We can use [Apache Commons Math](http://commons.apache.org/math/) to perform some statistical tests. It is an interesting Java library, not really focused on statistics, but pretty easy to use and that luckily contains ANOVA. 


    
    
    import java.util.ArrayList;
    import java.util.List;
    
    import org.apache.commons.math.*;
    
    public class FraudDetector {
        private static final double SIGNIFICANCE_LEVEL = 0.001; // 99.9%
    
        public static void main(String[] args) throws MathException {
            double[][] observations = {
               { 150.0, 200.0, 180.0, 230.0, 220.0, 250.0, 230.0, 300.0 },
               { 200.0, 240.0, 220.0, 250.0, 210.0, 190.0, 240.0 },
               { 100.0, 130.0, 150.0, 180.0, 140.0, 200.0, 110.0, 120.0 },
               { 200.0, 230.0, 150.0, 220.0, 210.0 }
            };
    
            final List<double[]> classes = new ArrayList<double[]>();
            for (int i=0; i<observations.length; i++) {
                classes.add(observations[i]);
            }
    
            OneWayAnova anova = new OneWayAnovaImpl();
            boolean rejectNullHypothesis =
                        anova.anovaTest(classes, SIGNIFICANCE_LEVEL);
    
            if (rejectNullHypothesis) {
                System.out.println("Significant differences were found");
            }
        }
    }
    



The [question I asked in stackoverflow](http://stackoverflow.com/questions/2326399/efficient-algorithm-for-detecting-different-elements-in-a-collection) includes some discussion and additional information about how to determine the rare cases.

One final thought. Despite I am not an expert in this field, I definitely think we should study more about statistics at the University.
