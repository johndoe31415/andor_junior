# andor_junior
Andor Junior is a simplified game of [Legends of
Andor](https://legenden-von-andor.de/), targeted towards kids. In the base game
there are four differented characters (Dwarf, Mage, Rogue, Paladin) which have
different strengths and weaknesses reflected by their character attributes.
They each have individual dice, different counts of dice and different amounts
of maximum moves they can perform. Hence, some classes are better at fighting while
others excel at searching tasks.

Like any reasonable computer scientist, the question that pops up immediately
is: Can we quantify this?

Of course we can. This is an excellent example of using [Markov
Matrices](https://en.wikipedia.org/wiki/Stochastic_matrix) from the probability
graph. The reason for this is that fights can last multiple rounds and some
dice get "used up" (when you succeed partially battling a Gor). Hence we need to keep track of:

  - The remaining number of moves (dictated by the character attribute of solar
    discs plus one because they may carry a special item that increases their
    solar disc count by one)
  - The remaining number of dice
  - The remaining number of enemies (Gors); this can be either 2 or 3 in the game.

## Implementation
The implementation does not aim to be efficient, but to show easily how to
transform a state machine with probabilities into a Markov Stochastic Matrix.
All computation are self-contained, no dependencies needed besides Python3.

## Results
Results first. Combined probabilities of fighting a 2-Gor/3-Gor or finding by
throwing a torch in n moves:

```
Held: Zwerg*in
Züge         2-Gor        3-Gor        Fackel
1            26%          4%           70%       
2            58%          17%          91%       
3            79%          35%          97%       
4            90%          52%          99%       
5            95%          65%          100%      
6            98%          76%          100%      
7            99%          83%          100%      

Held: Magier*in
Züge         2-Gor        3-Gor        Fackel
1            50%          50%          17%       
2            75%          75%          31%       
3            88%          88%          42%       
4            94%          94%          52%       
5            97%          97%          60%       
6            98%          98%          67%       
7            99%          99%          72%       

Held: Bogenschütz*in
Züge         2-Gor        3-Gor        Fackel
1            26%          4%           42%       
2            58%          17%          67%       
3            79%          35%          81%       
4            90%          52%          89%       
5            95%          65%          94%       
6            98%          76%          96%       
7            99%          83%          98%       
8            100%         89%          99%       
9            100%         92%          99%       
10           100%         95%          100%      

Held: Krieger*in
Züge         2-Gor        3-Gor        Fackel
1            69%          31%          52%       
2            95%          74%          77%       
3            99%          92%          89%       
4            100%         98%          95%       
5            100%         99%          97%       
6            100%         100%         99%       
7            100%         100%         99%      
```

Also note that when you compute this, the program emits a GraphViz graph for
each character and battle type:

```
$ dot -Tpdf -ohero_dwarf_2gor.pdf hero_dwarf_2gor.dot
```

Example:

![Dwarf vs. 2-Gor](https://raw.githubusercontent.com/johndoe31415/andor_junior/main/hero_dwarf_2gor.png)

Here, you can see the battle of a dwarf against a 2-Gor: it shows the amount of
moves ("Z"), dice ("W") and remaining Gors ("G"). For exmaple, when starting
out with 6 moves and 3 dice against a 2-Gor, you need to look at the "6 Z, 3 W,
2 G" node. There is a 26% chance that you win the battle in one move (note the
target node is green). With 30% probability you will not hit any Gor and with
44% probability you will slash one Gor (one dice used up, one Gor remaining).

## License
GNU GPL-3.
