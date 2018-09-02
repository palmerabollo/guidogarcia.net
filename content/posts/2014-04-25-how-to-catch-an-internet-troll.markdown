---
author: admin
date: 2014-04-25 16:19:37+00:00
draft: false
title: How to catch an Internet troll
type: post
url: /blog/2014/04/25/how-to-catch-an-internet-troll/
categories:
- Tecnología
tags:
- classification
- java
- lingpipe
- nlp
- statistics
- troll
---

Some weeks ago I carried out a social experiment (<del>dameunverso.com</del>, spanish) that consisted in writing a poem in a collaborative and anonymous way. This means that anyone can add a new verse to the poem without identifying themselves or leaving any metadata (no cookies, no IP address tracking, etc).

Our first trolls didn't take long to appear, mostly in the form of copyrighted material, spam and offensive contents. **Is it possible to automatically classify an anonymous verse as spammy?**

![Troll](http://i.blogs.es/e3f676/trollface/450_1000.jpg)




### Text classification



[LingPipe](http://alias-i.com/lingpipe/) is a powerful Java toolkit for processing text, free for research use under some conditions. I followed the [Text Classification Tutorial](http://alias-i.com/lingpipe/demos/tutorial/classify/read-me.html), to classify verses in one of these categories: **"spam" or "love"**.

The classifier I built uses 80% of the poem (already classified into "spam" or "love" categories by hand), as a training set to learn and build a language model. Then, it uses the remaining 20% of the poem (48 verses) for cross-validation of this model.

You can find the code in the Annex I, it is less than 50 lines of code.



### Classification results



The classification **accuracy is 75% ± 12.25%**, so we can say that our model performs better than a monkey at significance level of 0.05.


    
    
    Categories=[spam, love]
    Total Count=48
    Total Correct=36
    Total Accuracy=0.75
    95% Confidence Interval=0.75 +/- 0.1225
    
    Confusion Matrix
    Macro-averaged Precision=0.7555
    Macro-averaged Recall=0.7412
    Macro-averaged F=0.7428
    



It seems pretty promising and it can serve as inspiration but, to be honest, I don't think it is such a good model. With so few contributions to the poem, it is prone to overfitting, so it is probably learning to classify just our usual trolls that are not original whatsoever.

Moreover, we are not taking into account other factors that would greatly improve the results, such as the structure of the verse (length, number of words, etc), the relation between the verses (rhyme) or the presence of inexistent words and typos. If you want to further investigate, I suggest taking a look at [Logistic Regression](http://alias-i.com/lingpipe/demos/tutorial/logistic-regression/read-me.html), to build better models that also include these kind of factors.

On a practical note, if you ever plan to carry out a similar experiment, remember two rules. First, make it easier for you to revert vandalism than for the troll to vandalize your site. Second, don't feed the troll. They will eventually get tired.



### Annex I. Java Code




    
    
    String[] CATEGORIES = { "spam", "love" };
    int NGRAM_SIZE = 6;
    
    String textSpamTraining =
            "Ola ke Ase\n" +
            "censurator\n" +
            "...";
    
    String textLoveTraining =
            "Me ahogo en un suspiro,\n" +
            "miro tus ojos de cristal\n" +
            "...";
    
    String[] textSpamCrossValidation = {
            "os va a censurar",
            "esto es una mierda",
            "..."
    };
    
    String[] textLoveCrossValidation = {
            "el experimento ha revelado",
            "que el gran poeta no era orador",
            "y al no resultar como esperado",
            "se ha tornado en vil censor",
            "..."
    };
    
    // FIRST STEP - learn
    DynamicLMClassifier<NGramProcessLM> classifier =
            DynamicLMClassifier.createNGramProcess(CATEGORIES, NGRAM_SIZE);
    
    {
        Classification classification = new Classification("spam");
        Classified<CharSequence> classified =
            new Classified<CharSequence>(textSpamTraining, classification);
        classifier.handle(classified);
    }
    
    {
        Classification classification = new Classification("love");
        Classified<CharSequence> classified =
            new Classified<CharSequence>(textLoveTraining, classification);
        classifier.handle(classified);
    }
    
    // SECOND STEP - compile
    JointClassifier<CharSequence> compiledClassifier =
                    (JointClassifier<CharSequence>) 
                            AbstractExternalizable.compile(classifier);
    
    JointClassifierEvaluator<CharSequence> evaluator =
            new JointClassifierEvaluator<CharSequence>(
                    compiledClassifier, CATEGORIES, true);
    
    // THIRD STEP - cross-validate
    for (String textSpamItem: textSpamCrossValidation) {
        Classification classification = new Classification("spam");
        Classified<CharSequence> classified =
            new Classified<CharSequence>(textSpamItem, classification);
        evaluator.handle(classified);
    }
    
    for (String textLoveItem: textLoveValidation) {
        Classification classification = new Classification("love");
        Classified<CharSequence> classified =
            new Classified<CharSequence>(textLoveItem, classification);
        evaluator.handle(classified);
    }
    
    ConfusionMatrix matrix = evaluator.confusionMatrix();
    System.out.println("Total Accuracy: " + matrix.totalAccuracy());
    
    System.out.println(evaluator);
    
