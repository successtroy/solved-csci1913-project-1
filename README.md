Download Link: https://assignmentchef.com/product/solved-csci1913-project-1
<br>
A <em>grammar</em> is a set of rules for generating strings. In this project, you will write a Python program that generates random strings using a grammar. For example, a version of this program generated the following simple English sentences as Python strings.

the cat bit the boy .   the cat kissed the dog and the boy chased the boy .

the cat chased the dog and the girl bit the boy but the girl chased the cat .   the girl chased the dog .

the boy kissed the girl and the cat kissed the girl .

Perhaps such grammars could write children’s books automatically. Seriously, however, more complex grammars have been used to generate random test inputs for programs, as a debugging aid.

<ol>

 <li><strong> Theory.</strong></li>

</ol>

To write this program, you must have a way to generate random integers. You must also know what a rule is, what a grammar is, and how rules and grammars work.

<strong>Random integers.</strong> No algorithm can generate truly random integers, but it can generate <em>pseudo-random</em> integers that seem random, even though they’re not. Python has its own random number generators, but for this project, you must implement your own.

The <em>Park-Miller algorithm</em> (named for its inventors) is a simple way to generate a sequence of pseudo-random integer terms. It works like this. Let <em>N</em>₀ be an integer called the <em>seed</em>. The seed is the first term of the sequence, and must be between 1 and 2³¹ − 2, inclusive. Starting from the seed, later terms <em>N</em>₁, <em>N</em>₂, … are produced by the following equation.

<em>N</em><em><sub>k</sub></em>+1 = (7<sup>5</sup> <em>N</em><em><sub>k</sub></em>) % (2<sup>31</sup> − 1)

Here 7⁵ is 16807, and 2³¹ is 2147483648. The Python operator % returns the remainder after dividing one integer by another. You will always get the same sequence of terms from a given seed. For example, if you start with the seed 101, then you will get a pseudorandom sequence whose first few terms are 1697507, 612712738, 678900201, 695061696, 1738368639, 246698238, 1613847356, and 1214050682. You could use this sequence to test if your random number generator works correctly.

Terms in the sequence may be large, but you can make them smaller by using the % operator again. If <em>N</em> is a term from the sequence, and <em>M</em> is an integer greater than 0, then <em>N</em> % <em>M</em> gives you an integer between 0 and <em>M</em> − 1, inclusive. For example, if you need an integer between 0 and 9, then you would write <em>N</em> % 10.

<strong>Grammars.</strong> The easiest way to explain a grammar is to show an example. This is the grammar that generated the simple sentences about boys, cats, dogs, and girls that appeared in the introduction.

<ul>

 <li><em>Noun</em> →  ‘cat’</li>

 <li><em>Noun</em> →  ‘boy’</li>

 <li><em>Noun</em> →  ‘dog’</li>

 <li><em>Noun</em> →  ‘girl’</li>

 <li><em>Verb</em> →  ‘bit’</li>

 <li><em>Verb</em> →  ‘chased’</li>

 <li><em>Verb</em> →  ‘kissed’</li>

 <li><em>Phrase</em> →  ‘the’ <em>Noun Verb</em> ‘the’ <em>Noun</em></li>

 <li><em>Story</em> →  <em>Phrase</em></li>

 <li><em>Story</em> →  <em>Phrase</em> ‘and’ <em>Story</em></li>

 <li><em>Story</em> →  <em>Phrase</em> ‘but’ <em>Story </em>12                  <em>Start</em>  →  <em>Story</em> ‘.’</li>

</ul>

Each line is a <em>rule,</em> identified by a number, so this grammar has 12 rules. The names in italics, like <em>Noun, Verb,</em> and <em>Phrase,</em> are called <em>nonterminals.</em> The strings in quotes, like ‘girl’, ‘the’ and ‘.’, are called <em>terminals.</em> In each rule, the arrow ‘→’ means <em>may be replaced by</em>. As a result, rule 01 says that the nonterminal <em>Noun</em> may be replaced by the terminal ‘cat’. Similarly, rule 10 says that the nonterminal <em>Story</em> may be replaced by the nonterminal <em>Phrase</em>, followed by the terminal ‘and’, followed by the nonterminal <em>Story</em>.       To generate strings from the grammar, you play a little game. The game always begins with the nonterminal <em>Start</em>. The object of the game is to use the rules to get rid of the nonterminals, replacing them by terminals. If you can do that, then the terminals left over at the end are concatenated to produce a string generated by the grammar. For example, you begin with <em>Start</em> and use rule 12 to replace it, like this.

<em>Story</em> ‘.’

Now you have a new nonterminal, <em>Story</em>, to get rid of. According to rule 10, you can replace <em>Story</em> by <em>Phrase</em> ‘and’ <em>Story</em>, so you get this.

<em>Phrase</em> ‘and’ <em>Story</em> ‘.’

You can then use rule 09 to replace <em>Story</em> by <em>Phrase</em>.

<em>Phrase</em> ‘and’ <em>Phrase</em> ‘.’

You use rule 08 to replace the first <em>Phrase</em>, so you get this.

‘the’ <em>Noun Verb</em> ‘the’ <em>Noun</em> ‘and’ <em>Phrase</em> ‘.’

According to rule 01, you can replace the first <em>Noun</em> by ‘cat’, and according to rule 02, you can replace the second <em>Noun</em> by ‘boy’.

‘the’ ‘cat’ <em>Verb</em> ‘the’ ‘boy’ ‘and’ <em>Phrase</em> ‘.’

And according to rule 06, you can replace <em>Verb</em> by ‘chased’.

‘the’ ‘cat’ ‘chased’ ‘the’ ‘boy’ ‘and’ <em>Phrase</em> ‘.’

Continuing the game, you use rule 08 again to replace <em>Phrase.</em>

‘the’ ‘cat’ ‘chased’ ‘the’ ‘boy’ ‘and’ ‘the’ <em>Noun Verb</em> ‘the’ <em>Noun</em> ‘.’

You use rule 02 to replace the first <em>Noun</em> by ‘boy’, and use rule 01 to replace the second <em>Noun</em> by ‘cat’. Finally, you use rule 07 to replace <em>Verb</em> by ‘kissed’.

‘the’ ‘cat’ ‘chased’ ‘the’ ‘boy’ ‘and’ ‘the’ ‘boy’ ‘kissed’ ‘the’ ‘cat’ ‘.’

At this point, you’ve eliminated all the nonterminals, so you’ve won the game. If you concatenate all the strings together, separated by blanks, then you get a string generated by the grammar, like this:

‘the cat chased the boy and the boy kissed the cat .’

By the way, there are many different kinds of grammars, each with different kinds of rules. The grammars used for this project are called <em>context-free grammars,</em> in which each rule has a single nonterminal on the left side of the arrow ‘→’.

<ol start="2">

 <li><strong> Implementation.</strong></li>

</ol>

The grammar game described in the previous section is so simple that a short Python program can play it. You must implement such a program for this project. Your program must use three Python classes, called Random, Rule, and Grammar. They are all separate classes: none of them extends any of the others.

<strong>The class </strong><strong>Random.</strong> The first class must be called Random, and it must implement the Park-Miller algorithm. It must have the following methods. They must have the same parameters as described here, and they must work as described here.

__init__(self, seed)

(5 points.) Initialize an instance of Random so it generates the sequence of pseudo-random integers that begin with seed. You may assume that seed is an integer in the proper range for the Park-Miller algorithm to work.

next(self)

(5 points.) Generate the next random integer in the sequence, and return it.

choose(self, limit)

(5 points.) Call next to obtain a random integer. Then compute a new integer between 0 and limit − 1 from it, as described in the previous section. Return the new integer.

For efficiency, your Random class must not compute big numbers like 7⁵ and 2³¹ over and over again. Either compute them only once, and store them in variables, or else write them as constants, so you don’t have to compute them at all.

All the methods in Random must be very short, just one to three lines each. If your methods are longer than that, then you do not understand what you are doing, so you should ask for help.

<strong>The class </strong><strong>Rule.</strong> The second class must be called Rule, and its instances must represent a rule from a grammar. It must have the following methods. They must have the same parameters as described here, and they must work as described here.

__init__(self, left, right)

(5 points.) Each instance of Rule must have three variables. The variable self.left must be initialized to the string left. The variable self.right must be initialized to the tuple of strings right. The variable self.count must be initialized to the integer 1: it is used by the class Grammar to help choose rules.

__repr__(self)

(5 points.) Return a string of the form ‘<em>C</em> <em>L</em> –&gt; <em>R</em>₁ <em>R</em>₂ ⋯ <em>R</em><em>ₙ</em>‘, where <em>C</em> is self.count, <em>L</em> is self.left, and <em>R</em>₁ <em>R</em>₂ ⋯ <em>R</em><em>ₙ </em>are the elements of self.right. For example, if you create a rule by calling the constructor:

Rule(‘Story’, (‘Phrase’, ‘and’, ‘Story’)) then its __repr__ method must return the string:

‘1 Story –&gt; Phrase and Story’

In other words, the string must look something like a rule.

Python calls the method __repr__ automatically when it prints an instance of the class Rule. This method is not used by the rest of this program, but it is helpful for debugging. If the __repr__ method was not defined, then Python would print an instance of Rule as a string of gibberish, and you would not be able to tell what rule it was. The variable self.left is used only by __repr__.

<strong>The class </strong><strong>Grammar.</strong> The third class must be called Grammar, and it must implement a grammar using rules like the ones described in the previous section. It must have the following methods. They must have the same parameters as described here, and they must work as described here.

__init__(self, seed)

(5 points.) Initialize an instance of Grammar. It must make an instance of the random number generator Random that uses seed. It must also make a dictionary that stores rules. The dictionary must be initially empty.

rule(self, left, right)

(5 points.) Add a new rule to the grammar. Here left is a string. It represents the nonterminal on the left side of the rule. Also, right is a tuple whose elements are strings. They represent the terminals and nonterminals on the right side of the rule.

Find the value of left in the dictionary. If there is no such value, then let the value of left in the dictionary be a tuple whose only element is Rule(left, right). However, if there is such a value, then it will be a tuple. Add Rule(left, right) to the end of that tuple.

generate(self)

(5 points.) Generate a string. If there is a rule with the left side ‘Start’ in the dictionary, then call generating with the tuple (‘Start’,) as its argument, and return the result. If there is no such rule, then raise an exception, because you cannot generate strings without a rule for ‘Start’.

generating(self, strings)

(10 points.) This method, along with select, does most of the work for this program. The parameter strings is a tuple whose elements are strings. The strings represent terminals and nonterminals.

Initialize a variable called result to ”, the empty string. It holds the result that will be returned by this method. Then use a loop to visit each string in strings.

If the visited string is not a key in the dictionary, then it is a terminal. Add it to the end of result, followed by a

blank ‘ ‘.

If the visited string is a key in the dictionary, then it is a nonterminal. Call select on the string to obtain a tuple of strings. Then call generating recursively on that tuple of strings, to obtain a new string. Add the new string to the end of result, without a blank at the end.

Continue in this way until all the strings in strings have been visited. At that point, result will be a string generated by the grammar. Return result.

select(self, left)

(10 points.) This method, along with generating, does most of the work for this program. It chooses a rule at random whose left side is the string left. This happens in several steps:

<ol>

 <li>Set the variable rules to be the tuple of all rules with left on their left sides (from the dictionary). Set the variable total to the sum of all the count variables in rules.</li>

 <li>Set the variable index to an integer chosen at random. It must be greater than or equal to 0, but less than total.</li>

 <li>Visit each rule in rules, one at a time. Subtract the rule’s count variable from index. Continue in this way until index becomes less than or equal to 0. Set the variable chosen to the rule that was being visited when this occurred. (As a result, rules with large count variables are more likely to be chosen than rules with small count )</li>

 <li>Add 1 to the count variables of all rules in rules, other than chosen. (This makes it more likely that a rule other than chosen will be selected later—giving a wider range of random strings.)</li>

</ol>

Finally, return the right variable of chosen, a tuple of strings.

You may want to write helper functions or methods for the class Grammar, especially for Grammar’s method select.

<ol start="3">

 <li></li>

</ol>

Here is an grammar G that you can use to test your program. It has the rules of the example grammar that was described earlier.

G = Grammar(101)

G.rule(‘Noun’,   (‘cat’,))                                #  01  G.rule(‘Noun’,   (‘boy’,))                                #  02  G.rule(‘Noun’,   (‘dog’,))                                #  03  G.rule(‘Noun’,   (‘girl’,))                               #  04  G.rule(‘Verb’,   (‘bit’,))                                #  05  G.rule(‘Verb’,   (‘chased’,))                             #  06  G.rule(‘Verb’,   (‘kissed’,))                             #  07  G.rule(‘Phrase’, (‘the’, ‘Noun’, ‘Verb’, ‘the’, ‘Noun’))  #  08  G.rule(‘Story’,  (‘Phrase’,))                             #  09  G.rule(‘Story’,  (‘Phrase’, ‘and’, ‘Story’))              #  10  G.rule(‘Story’,  (‘Phrase’, ‘but’, ‘Story’))              #  11

G.rule(‘Start’,  (‘Story’, ‘.’))                          #  12

In this example, G’s random number generator is initialized with the seed 101. If you call G.generate() five times, then you should get the same five sentences that appear in the introduction. You may want to run additional examples to make sure that your program works correctly.