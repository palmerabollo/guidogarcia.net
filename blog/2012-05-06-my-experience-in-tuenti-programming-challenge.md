---
author: admin
date: 2012-05-06 18:29:45+00:00
draft: false
title: My experience in the 2nd Tuenti Programming Challenge
type: post
url: /blog/2012/05/06/my-experience-in-tuenti-programming-challenge/
categories:
- Software
- Tecnología
tags:
- contest
- tuenti
---

This week I have been participating in the 2nd edition of the [Tuenti Programming Challenge](https://contest.tuenti.net/). I felt a little rusty on my return to top-level competition, but despite I started the competition three days late, I was able to reach level 14 ([stats](https://contest.tuenti.net/Stats)). Not so bad.

The problems I prefer are those with an obvious brute force solution, but that can take advantage of a particular algorithm or data structure. Most of the problems in this edition belong to this category except, perhaps, the [challenge 12](https://contest.tuenti.net/Questions?id=12) which I did not like because I found it too much tricky.



### The crazy croupier



The challenge I enjoyed the most was the number 13, [the crazy croupier](https://contest.tuenti.net/Questions?id=13). It is kind of a classical problem with minor variations, where you have to determine how many shuffles you need in a deck of N cards in order to come back to the original position if you cut it at the position L.

The **easy -[brute force](http://2.bp.blogspot.com/_3IjRgoGWUBo/S-x57diOByI/AAAAAAAAAiA/dTacUrhoTAU/s1600/nukes.jpg)- solution** is to iterate and count until the cards are in their original positions, which can be a time-consuming task when the number of the cards is big (up to 10^6 in this case).

The second approach is to **convert it to a permutations problem**. Once you know  where the card 1..N is located after the first shuffle, you can determine the number of shuffles each individual card needs to come back to its location. The least common multiple of the individual results is the total number of shuffles needed.



### Show me the code



Here it is ([download](https://gist.github.com/gists/2623553/download)). I do not know how many participants chose Java to solve the problems. What I have seen so far are solutions in Python ([here](https://github.com/robermorales/Tuenti-Challenge2012-solutions/tree/master/robermorales), [here](https://github.com/TheOm3ga/SolucionesTuenti2012)) that seem more compact and PHP ([here](https://github.com/ricardclau/tuenti-contest/tree/master/2012)). It is a pity that the official ranking does not show the execution times :)


    
    
    /**
     * Crazy Croupier - 2nd Tuenti Challenge - 12
     * 
     * Example:
     * Number of cards: N = 10
     * Number of cards in the first set: L = 3
     * Cards: 10 1 4 2 8 5 6 7 3 9
     * 
     * First set: 10 1 4
     * Second set: 2 8 5 6 7 3 9
     * 
     * Shuffled set: 4 9 1 3 10 7 6 5 8 2
     */
    public static void main(String[] args) {
      Scanner scanner = new Scanner(System.in);
    
      // read number of cases
      int cases = Integer.parseInt(scanner.nextLine());
    
      for (int i=1; i<=cases; i++) {
        // read N L, for example 10 6
        String line = scanner.nextLine();
        String[] arguments = line.split(" ");
    
        // N = cards in the deck
        int N = Integer.parseInt(arguments[0]);
    
        // L cards in the first bunch (where to cut the deck)
        int L = Integer.parseInt(arguments[1]);
    
        long result = processCase(N, L);
        System.out.printf("Case #%d: %d\n", i, result);
      }
    }
    
    /**
     * Returns the number of shuffles required to return the deck
     * to its original order.
     * The algorithm will calculate the number of iterations that
     * each individual card need to come back to its position. The
     * solution will be the least common multiple (lcm) of the
     * individual results.
     */
    private static long processCase(int n, int cut) {
      int[] deck = new int[n]; // first deck shuffling result
    
      shuffleDeck(n, cut, deck);
    
      // cache source -> target positions for O(1) access
      final Map<Integer, Integer> permutations = new HashMap<Integer, Integer>();
      for (int i=0; i<deck.length; i++) {
        permutations.put(deck[i], i+1);
      }
    
      // cache to avoid multiple lcd calculations of the same num, O(1) access
      Set<Long> calculatedLcm = new HashSet<Long>();
      long lcm = 0;
      for (int i=0; i<deck.length; i++) {
        long numberPermutations = 1; // we already did the first shuffling
        int currentPosition = i+1;
    
        // still no at the original position
        while (currentPosition != deck[i]) {
          numberPermutations++;
          currentPosition = permutations.get(currentPosition);
        }
    
        if (calculatedLcm.contains(numberPermutations) == false) {
          lcm = lcm(lcm, numberPermutations);
          calculatedLcm.add(numberPermutations);
        }
      }
    
      return lcm;
    }
    
    /**
     * Shuffles the deck one time according to the algorithm.
     */
    private static void shuffleDeck(int n, int cut, int[] deck) {
      int min = Math.min(cut, n-cut); // number of cards shuffled
    
      // shuffle two bunch of cards
      for (int i=0; i<min; i++) {
        deck[2 * i] = cut - i;   // 1st bunch
        deck[2 * i + 1] = n - i; // 2nd bunch
      }
    
      // put the rest of the cards in proper order, at the end
      for (int i=0; i<n - 2 * min; i++) {
        if (cut >= n / 2) { // first bunch is bigger
          deck[n - 1 - i] = i + 1;
        } else { // second bunch is bigger or equal
          deck[2 * min + i] = n - min - i;
        }
      }
    }
    
    /**
     * Least common multiple. Probably a more efficient approach can be
     * found but it is good enough.
     */
    private static long lcm(long a, long b) {
      if (a == 0 || b == 0) {
        return Math.max(a, b);
      }
      return a * b / gcd(a, b);
    }
    
    /**
     * Greatest common divisor, see also {@link BigInteger#gcd(BigInteger)}
     */
    private static long gcd(long a, long b) {
      long mod = a % b;
      return mod == 0 ? b : gcd(b, mod);
    }
    



What do you think about it?



### Talent is out there



I like these competitions (Google Code Jam, ACM ICPC, etc) a lot and I think it is a great (and cheap) opportunity for technological companies to attract and recruit talent. There are a lot of great coders hidden out there.
