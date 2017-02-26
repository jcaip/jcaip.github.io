---
published: True

images:

    - url: /images/algs/Greedy/InterviewScheduling.png
      alt: Interval Scheduling
      title: Interval Scheduling
---
{ assign image = page.images[0] %}
{% include image.html image=image %}
### What are Greedy Algorithms?

**Greedy Algorithms** work by building the general solution one step at a time. The forego thinking about the future and take the best possible *step* each time until they have a complete solution.

### Interval Scheduling
![Interval](/images/algs/Greedy/InterviewScheduling.png)

### Mininium Spanning Trees
![MST](/images/algs/Greedy/ComputingMST.png)

## Proof Techniques
### Exchange argument
The exchange argument is true if for our function A and some input I the ouptut A(I),and some other output O,

we can produce a new solution A(I)' that is 

+ just as good as A(I) 
+ is more similar O than A(I) is 

In combination with proof by contradiction, we cn use this to prove our function is optimal.

Assume, our algorithm A is not correct.
There is some solution A(I) that is the closest possible solution to the optimal solution O. 

However, if we know the exchange argument was true, 
one could always modify A(I) to create A(I)', which is just as optimal, but more like O than A(I) is

Which is a contradiction because  we stated A(I) is the closest.
