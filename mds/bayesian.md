## Conditional probability

If A and B are events with P(B) > 0,
then the conditional probability of A given B, denoted by P(A|B), is defined as

$$ P(A|B) = \frac{P(A \cap B)}{P(B)}  $$

### Example 2.2.2

A standard deck of cards is shuffled well. Two cards are drawn randomly, one at a time without replacement. Let A be the event that the first card is a heart ❤, and B the event that the second card is red. Find P(A|B) and P(B|A)


```python
# A intersection B
# 13 -> heart cards
# 25 -> 13*2 = 26 red cards, 26-1 + 25, -1 because the previous hearts card is red too
# 52 -> total cards
# 51 -> 52-1, because one card is drawn, so:

intersection = (13*25)/(52*51)
# prob(B) = 1/2 -> because there is 50% of probabilities to draw a red card
p_b = 1/2

print('P(A|B):')
print(intersection/p_b)
```

    P(A|B):
    0.24509803921568626
    


```python
# prob(A) = 1/4 -> because ther is 50% of probabilities to draw a heart
p_a = 1/4
print('P(B|A):')
print(intersection/p_a)
```

    P(B|A):
    0.49019607843137253
    

### Example 2.2.5 

(Two children). Martin Gardner posed the following puzzle in the
1950s, in his column in Scientific American.

A) Mr. Jones has two children. The older child is a girl. What is the probability that
both children are girls?

B) Mr. Smith has two children. At least one of them is a boy. What is the prob-
ability that both children are boys?

A) Let's use truth tables (p AND q), assume that 1 is a girl, 0 isn't

| first child | second child | Result |
|-------------|--------------|--------|
| 1           | 0            | 0      |
| 1           | 1            | 1      |

then p = 1/2 = 0.5

B) The truth table here is

| first child | second child | Result |
|-------------|--------------|--------|
| 1           | 0            | 0      |
| 0           | 1            | 0      |
| 1           | 1            | 1      |
| 1           | 1            | 1      |

But the two last rows are equal (redundant) so simplifying it

| first child | second child | Result |
|-------------|--------------|--------|
| 1           | 0            | 0      |
| 0           | 1            | 0      |
| 1           | 1            | 1      |

the p = 1/3

#### Bayes method

There's no reason to use Bayes as we saw above but anyways...

Intersection or P(both girls|elder is a girl) = 0.25 cos the chances are

| first child | second child | Result |
|-------------|--------------|--------|
| 0           | 0            | 0      |
| 0           | 1            | 0      |
| 1           | 0            | 0      |
| 1           | 1            | 1      |

P(elder is a girl in any case) = 1/2 


```python
#Let's code
#Case A
intersection = 0.25
p_b = 0.5

print('P(A|B):')
print(intersection/p_b)
```

    P(A|B):
    0.5
    


```python
#Case B
#intersection in this case is P(both girls|at least one girl)
intersection = 0.25
#P(B) -> at least one girl, i.e. 3/4
p_b = 0.75

print('P(A|B):')
print(intersection/p_b)
```

    P(A|B):
    0.3333333333333333
    

### Theorem

For any events A and B with positive probabilities,

$$ P(A \cap B) = P(B)P(A|B) = P(A)P(B|A) $$


### Bayes' rule

$$ P(A|B) = \frac{P(B|A)P(A)}{P(B)}  $$

### Example 2.3.9

(Testing for a rare disease) 

A patient named Fred is tested for a disease called conditionitis, a medical condition that affects 1% of the population.
The test result is positive, i.e., the test claims that Fred has the disease. Let D be the event that Fred has the disease and 
R be the event that he tests positive.

Suppose that the test is "95% accurate"; there are different measures of the accuracy of a test, but in this problem it is assumed to mean that P(R|D) = 0.95 and P(Rc|Dc) = 0.95. The quantity P(R|D) is known as the sensitivity or true positive
rate of the test, and P(Rc|Dc) is known as the specificity or true negative rate. Find the conditional probability that Fred has conditionitis, given the evidence provided by the test result.


```python
#p_d -> infection probability
#p_nod -> no infection probability
#p_r_with_d -> infected with positive result probabilty
#p_nor_with_d -> no infected with negative result probability
#p_r_with_nod -> no infected, false positive

p_d = 0.01
p_nod = 0.99
p_r_with_d = 0.95
p_nor_with_nod = 0.95
p_r_with_nod = 0.05
```

Using Bayes'rule

$$ P(R|D)P(D) = P(D|R)P(R) \Rightarrow P(D|R) = \frac{P(R|D)P(D)}{P(R)}$$ 

We need to know P(R) first, which is 

$$ P(R) = P(R|D)P(D)+P(R|Dc)P(Dc) $$


```python
p_r = (p_r_with_d*p_d)+(p_r_with_nod*p_nod)
print("probability of positive result (sum of true positive + false positive)")
print(round(p_r, 3))
```

    probability of positive result (sum of true positive + false positive)
    0.059
    


```python
print("probability of infection:")
p_d_with_r = (p_r_with_d*p_d)/p_r
print(round(p_d_with_r, 3))
```

    probability of infection:
    0.161
    

### 2.10 Simulating the frequentist interpretation (in Python instead R)

Let's do the exercise 2.2.5 based on a large number n of repetitions. So:


```python
import random
sample = 10000
#1 stands for girl, 2 for boy
child1 = [random.randint(1, 2) for x in range(sample)]
child2 = [random.randint(1, 2) for x in range(sample)]
two_childs_are_girls = 0
first_child_is_girl = 0

for i in range(sample):
    if child1[i]==1 and child2[i]==1:
        two_childs_are_girls += 1
        
    if child1[i]==1:
        first_child_is_girl += 1

print(first_child_is_girl)
print(two_childs_are_girls)
print(two_childs_are_girls/first_child_is_girl) #must be 0.5 approx
```

    4957
    2457
    0.4956626992132338
    


```python
two_childs_are_girls = 0
one_child_is_girl = 0

for i in range(sample):
    if child1[i]==1 and child2[i]==1:
        two_childs_are_girls += 1
        
    if child1[i]==1 or child2[i]==1:
        one_child_is_girl += 1

print(one_child_is_girl)
print(two_childs_are_girls)
print(two_childs_are_girls/one_child_is_girl) #must be 0.33  approx
```

    7419
    2457
    0.3311767084512738
    
