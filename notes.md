Finding a job tool - to manage your followups and interviews and chats and the like. 
- can share data about how long responses and the like. 
Squat form checker tool / CV for home fitness. 
NLP tool to analyze comments and the like. 

# Making the black box effective

Something about quantiles, pinball loss (one line of slope $1-\alpha$ and one of $\alpha$).

Conformalied Quantile Regression, works very well. 

Get information from weights of a neural network.

# Differential Learning

Go through regions of 

Dynamic distances between tasks, reachability.

reachability given by a static part (task distance) and a dynamic part (which is the critical period). 

task2vec: or something

# Advanced MCMC
Rejection sampling doesn't work for high dimension - too many rejections

1. Ensembe Rejection sampling - sample N iid from q and then sample X from the posterior.
- compute upper bound, but practically useless

2. Draw N samples for t from 1 to T, use to build state sequence, and then use this as an approximation of $\pi$.
- can be used to sample prosterior of state-space models.

# Michael Jordan

markets - interconnected web of decisions/sequences of decisions.
divisions across a network , over time.
decisions - scarcity and competition. 

Econometrics.  Matching Markets

Regret-Minimization Algorithms. 
- pick arm with upper confidence bound:
- logarithmic regret bound
- Semantic segmentation GANs shit.
- M-ary and Mode GAN


# CS130 Notes

Waterfall method 
- Requirements Engineering
- Design
- Implementation
- Testing
- Evolution

UML is meant to be optional, open to interpretation and extension. 

Static modeling in UML:
    - Class diagrams, model fixed, code-level relationships
Behavioral modeling:


use case diagrams
- actors that shown by stick figures
- use cases that are ovals
- edges from actor to use case shows association
- use cases have relationships to each other
    - inclusion
    - generalization/specialization
    - extension expresses an exceptional variation

Statechart diagrams 
- show state and see transitions between state

Class diagrams
- models the static relationsips between components of a system
- single UML model can have many class diagrams
- multiplicity of the class is specified in the upper right corner
- Each box is a class, name, fields, and then methods
- Class relationships
    - dependency: weakest relationship, class A uses class B
    - association: stronger relationship, class A has a class B
    - aggregation: string association, class A owns class B (lifetime association)
    - composition: class A is made up of class B
    - generalization: shows inheritance, A subclass B has an is a relationship with superclass A
    - realization: used to show subtyping (class A implements interface B)
 - Preconditions, postconditions, and body conditions. 
 - template class allows a developer to design a class without specifying the exact type


Sequence Diagrams - focus on communication between elements

alt - alternative fragment for mutual exclusion conditional logic expressed in the gaurds
loop - loop fragment while guard is true
opt - optional fragment that executes if guard is true
par - parallel fragments that execute in parallel 


## Information Hiding

- low coupling, reduce the dependencies between modules
- high cohesion - a single concept is represented in a single place. 
- separation of concerns - a single concern is easily separated from the rest of the concerns.

module - self contained piece of code, an independent work assignment
IH - anticipated changes affect modules in an isolated and independent way. 


Functional decomposition - each module corresponds to each step in a flow chart, data representation is shared knowledge
IH - each module corresponds to something that is likely to change, interface definitions reveal as little as possible

Interface of a module should be about design decisions that are unlikely to change. 
Implementation of an interface : should be about secrets


## Design Patterns
Strategy  - defines a family of algorithms, encapsulates each one, and makes them interchangeable at runtime. 
```java
FlyBehavior fo;
fo.fly();
```

Observer - defines a one-to many dependency between objects so that one object changes state, all of its dependents are notified and updated automaticallly. 

Observers can be added at any time and can be reused or modified without impacting the others. 


Mediatior - all objects talk to the mediator instead of to each others

FactoryMethod - defines an interface for creating an object but lets subclasses decide which class to instantiate. 

```java
SimplePizzaFactory factory = new SimplePizzaFactory();
PizzaStore store = new PizzaStore(factory);
Pizza pizza = store.orderPizza("cheese");
```

makes it easy to add a new product type, a new creator, or change the conditions under which different objects are created. Client applications do not know individual concrete types of objects being created. 



AbstractFactoryMethod - provides an interface cfor creating a family of related or dependent objects without specifying their concrete classes. 

abstract factory includes a factory method pattern. 

Singleton - many objects we only want one of
```java
public class Singelton {
    private class Singleton uniqueInstance = null;

    private Singleton() {}

    public static Singleton getInstance() {
        if (uniqueInstance == null)
            uniqueInstance = new Singleton();
        else
            return uniqueInstance
    }
}
```

But this is not thread safe - use eager instantiation or double checked locking. 


Command - encapsulates requests as an object, letting you parameterize other objects with different requests, queue and log requests, and support undoable operations. 
mediator is an intermediate where multiple colleagues communicate, but command is an interface for an action that encapsulates an underlying device. 

Note device dependent, devices come and go. 

```java

```

Adaptor - converts the interface of a class into another interface the clients expect. 

```java 
public class TurkeyAdapter implements Duck {
    Turkey turkey;

    public TurkeyAdapter(Turkey turkey) {
        this.turkey = turkey;
    }

    public void quack() {
        turkey.gobble();
    }

    public void fly() {
        for(int i=0; i < 5; i++) {
            turkey.fly();
        }
    }
}
```

Facade - create interface for set of objects or complex subsystem.
Facade seeks to simplify, adapter seeks to convert. 

Observer - 1:n
Mediator n:n

TemplateMethod - abstraction of a commmon procedure/skeleton/workflow. 

Let subclasses redefine certain steps of an alg without changing alg structure. 

```java
public abstract class Thing {

    // final means subclasses cannot override
    final void common_procedure(){
        step1();
        varying_step2();

        if (test)
            step3();
        else
            barying_step3();
    }

    void step1();
    abstract void varying_step2();'
    abstract void varying_step3();'

}


public class ConcreteThing {
    public void varying_step2(){
        //some implementation
    }
    public void varying_step3(){
        //some implementation
    }
}

public class Client {
    void fun(){
        // new or from somewhere else
        Thing t = new ConcreteThing();
        t.common_procedure()
    }
}
```


-subclassing: interactive: reusing an implementation from a superclass 
-subtyping: polymorphism: declaring an interface

State pattern mechanics:
    - encapsulate the varying behavior
    - create a new class for every state
    - localize the behavior for each state
    - makes introducing new code clean (think about how each state changes)

```java
public class GumballMachine {
    State soldOutSatate;
    State noQuarterState;
    State hasQuarterState;
    State soldState;
    State state = soldOutState;
    int count = 0;

    GumballMachine(){
        soldOutState = new State(this);
    }
}

public class SoldState implements State {
    GumballMachine gumballMachine;

    public SoldState(GumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    public void dispense() {
        gumballMachine.releaseBall();
        gumballMachine.setState(gumballMachine.getNoState());
    }
}
```

State allows an object to alter it's behavior when it's internal state changes. The object will appear to change its class. 
but maybe some class explosion problem or brittle interfaces.

Flyweight pattern - allows one instance of a class can be reused to provide many virtual instances. 


Math for Computer Science:

Logic
Counting 
Probability
Discrete Math
Linear Algebra


Blend engineering, experience wrote ETL pipeliens, spark airflow. 

NLP model for detecting comments. 

NLP research, good embeddings and also k-cliques for topics. 

Try new things. 

Is this tech possible, what makes this a good time?

What are the biggest technical problems you face?



# Serenade


Use contrastive learning for knowledge distillation. 

The model is trained to predict the softmax outputs but nobody cares about MLM loss performance - nobody actually uses language models to predict the next work or a masked token. 

People use it as a representation for downstream tasks. 

So our model does not have to do well on MLM loss, it just has to be a just as good of a representation. 

So there's two aspects here: 
1. How can I reduce the dimensionality of embeddings and to what degreee?
2. Can I train a simpler model using this method?
