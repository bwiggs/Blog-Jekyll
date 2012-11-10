--- 
wordpress_id: 289
layout: post
title: JavaScript Closures
date: 2011-12-26 19:12:19 -07:00
wordpress_url: http://www.bwigg.com/?p=289
---
Javascript closures can be confusing at first. Rest assuerd, they're not that rediculous to comprehend, and hopefully this analogy will help drive home the concept.

## Wizard's First Rule Analogy

A man possess a secret book, which he cannot let anyone else know about. He decides the only way to keep the book truly secret is to memorize the entire thing and then bury the book so no one can find it. Later in his years he bears a son. Once his son grows old enough, the father digs up the book and begins having his son commit the entire book to memory. Years later, he has his son rewrite the entire book cover to cover from memory on paper. When the son successfully completes the task, and the father is satisfied that his son truly has the book memorized in it's entirety, the book and the copy of the book the son just wrote are thrown into a fire, completely eliminating any evidence that the book actually existed. A few years later the father dies, leaving the son on his own, with the book committed to memory.

This is similiar to how a javascript closure works. See the following code.

var father = new function(){

    // this is declared as a private variable
    var secretBook = 'Book of Secrets';

    this.bearSon = function(){
        return {
            whatWasTheBookYourFatherTaughtYou: function(){
                alert(secretBook);
            }
        };
    };
};


// the father won't tell anyone about the book he has
alert(father.secretBook); // undefined

// the father bears a son
son = father.bearSon();

// the father dies
father = undefined;

// the son still knows the book his father taught him
son.whatWasTheBookYourFatherTaughtYou(); // Book of Secrets

secretBook was declared inside the father function and is a private variable, inaccessible to anything outside the father function. The father function <em>closes</em> around the variables declared inside of it. Anything defined inside the father function has access to it, even once the father function has executed and been undefined. The bearSon method returns a new object that contains a function called whatWasTheBookYourFatherTaughtYou.
