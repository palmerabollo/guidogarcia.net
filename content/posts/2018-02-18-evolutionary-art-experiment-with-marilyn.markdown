---
author: admin
date: 2018-02-18 19:29:39+00:00
draft: false
title: Evolutionary art experiment with Marilyn
type: post
url: /blog/2018/02/18/evolutionary-art-experiment-with-marilyn/
categories:
- Tecnología
tags:
- experiment
- genetic algorithms
---

You know I like [evolutionary algorithms](http://guidogarcia.net/blog/2014/11/05/evolutionary-computation-%c2%b7-tefcon-2014/). In the past month I ran a small **art experiment reflecting the impact of brands and the harmful effect of advertisements in modern life**. The idea was to pick a cultural icon and recreate it using corporate logos only.

![Original image](http://guidogarcia.net/blog/wp-content/uploads/2018/02/marilyn.jpg)


I started with Marilyn Monroe and a genetic algorithm that combined a set of logos in a **mutation and a crossover** loop. The _mutation phase_ randomly picked some individuals (an individual is a specific combination of logos) and modified them, as nature would do, replacing some logos, resizing or rotating them. The _crossover phase_ chose pairs of individuals and produced a child individual from them.

The first image corresponds to the individual 46, after the algorithm had been running for **15 minutes** on a machine with 32 cores. Marylin wasn't there yet but everything looked good so far.

![Individual 46](http://guidogarcia.net/blog/wp-content/uploads/2018/02/output_individual_46.png)


After **12 hours**, Marilyn's eye and her eyebrow had already been replaced by an Evernote's elephant and a Facebook logo, as shown in the following image, that belongs to the individual 5449. Her lips seem to be McDonald's Golden Arches. The algorithm was slower than expected, but it looked promising.
<table align="center" >
<tbody >
<tr >

<td >![Individual 5449](http://guidogarcia.net/blog/wp-content/uploads/2018/02/output_individual_5449.png)

</td>

<td >![Original image](http://guidogarcia.net/blog/wp-content/uploads/2018/02/marilyn.jpg)

</td>
</tr>
</tbody>
</table>
Sadly, the fittest individual found after **24 hours **didn't look so good, so I cancelled the experiment. I didn't notice further improvements. The outline of Marilyn is also clear, but the result is way worse than I expected.
<table align="center" >
<tbody >
<tr >

<td >![Individual 8463](http://guidogarcia.net/blog/wp-content/uploads/2018/02/output_individual_8463.png)

</td>

<td >![Original image](http://guidogarcia.net/blog/wp-content/uploads/2018/02/marilyn.jpg)

</td>
</tr>
</tbody>
</table>
In hindsight, I wouldn't call the result a success. What do you say?



### How to run your own experiment


The good part of genetic algorithms is that you only need to code _what_ you want to achieve and not _how_ to achieve it. That's why the overall algorithm always look the same. In general you only need to **implement the fitness function**, that measures how good a given solution is. There are also different crossover algorithms. In this experiment I used a wheel probability selector, where the fittest individuals are more likely to be chosen for crossover and a [single point crossover](https://en.wikipedia.org/wiki/Crossover_(genetic_algorithm)#Techniques) technique.

    
    const population = ... // generate array of random individuals
    population
        .filter(individual => individual.fitness > MIN_FITNESS)
        .then(survivors => {
            // always kill the worst 20% individuals
            return survivors
                .sort((a, b) => b.fitness - a.fitness)
                .slice(0, survivors.length * 0.8);
        })
        .then(survivors => {
            // mutate phase
            const mutateCount = survivors.length * MUTATE_PROBABILITY;
            _.times(mutateCount, () => _.sample(survivors).mutate());
    
            // crossover phase with wheel selection
            const crossoverCount = survivors.length * CROSSOVER_PROBABILITY;
            _.times(crossoverCount, () => {
                const parent1 = wheel();
                const parent2 = wheel();
                const child = parent1.crossover(parent2);
                survivors.push(child);
            });
    
            return survivors;
        });
    


Node.js is not the best language to do cpu-intensive tasks, but I wanted to enjoy coding and I found [jimp](https://github.com/oliver-moran/jimp) and [sharp](https://github.com/lovell/sharp) very handy to compare and process images. Sharp is powered by [libvips](https://github.com/jcupitt/libvips) which makes it really efficient, but it wasn't enough due to the volume of images I needed to combine. Ideas and cpu-time to make the experiment succeed are welcome! Comments are open.
