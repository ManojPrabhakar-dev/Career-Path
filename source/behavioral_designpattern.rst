.. include:: include.rst

.. _behavioral_design_pattern:

###########################
Behavioral Pattern Concepts
###########################

**Behavioral patterns** are those patterns that are concerned with the interaction between the objects. 
The interaction between the objects should be in such a way that they are talking to each other and still are loosely coupled. 
The loose coupling is the key to **n-tier architecture**.

****************
Iterator Pattern
****************

**Iterator** is a behavioral design pattern that allows sequential traversal through a complex data structure without exposing its internal details.

.. image:: images/IteratorPattern_1.png
   :width: 700

* The **Iterator** defines an interface for accessing an Aggregate object and traversing elements within that Aggregate.


* The **ConcreteIterator** implements the Iterator interface and keeps track of its current position within the Aggregate.


* The **Aggregate** defines an interface for creating an Iterator object.


* The **ConcreteAggregate** implements the Iterator creation interface and returns a ConcreteIterator for that ConcreteAggregate.


.. code-block:: c#
    :caption: Iterator Pattern code example
        
        /// Our collection item.  Mostly because I'm a sucker for jelly beans.        
        class JellyBean
        {
            private string _flavor;

            // Constructor
            public JellyBean(string flavor)
            {
                this._flavor = flavor;
            }
                
            public string Flavor
            {
                get { return _flavor; }
            }
        }
        
        /// The aggregate interface       
        interface ICandyCollection
        {
            IJellyBeanIterator CreateIterator();
        }
       
        /// The ConcreteAggregate class       
        class JellyBeanCollection : ICandyCollection
        {
            private ArrayList _items = new ArrayList();

            public JellyBeanIterator CreateIterator()
            {
                return new JellyBeanIterator(this);
            }

            // Gets jelly bean count
            public int Count
            {
                get { return _items.Count; }
            }

            // Indexer
            public object this[int index]
            {
                get { return _items[index]; }
                set { _items.Add(value); }
            }
        }
       
        /// The 'Iterator' interface      
        interface IJellyBeanIterator
        {
            JellyBean First();
            JellyBean Next();
            bool IsDone { get; }
            JellyBean CurrentBean { get; }
        }
      
        /// The 'ConcreteIterator' class       
        class JellyBeanIterator : IJellyBeanIterator
        {
            private JellyBeanCollection _jellyBeans;
            private int _current = 0;
            private int _step = 1;

            // Constructor
            public JellyBeanIterator(JellyBeanCollection beans)
            {
                this._jellyBeans = beans;
            }

            // Gets first jelly bean
            public JellyBean First()
            {
                _current = 0;
                return _jellyBeans[_current] as JellyBean;
            }

            // Gets next jelly bean
            public JellyBean Next()
            {
                _current += _step;
                if (!IsDone)
                    return _jellyBeans[_current] as JellyBean;
                else
                    return null;
            }

            // Gets current iterator candy
            public JellyBean CurrentBean
            {
                get { return _jellyBeans[_current] as JellyBean; }
            }

            // Gets whether iteration is complete
            public bool IsDone
            {
                get { return _current >= _jellyBeans.Count; }
            }
        }

        static void Main(string[] args)
        {
            // Build a collection of jelly beans
            JellyBeanCollection collection = new JellyBeanCollection();
            collection[0] = new JellyBean("Cherry");
            collection[1] = new JellyBean("Bubble Gum");
            collection[2] = new JellyBean("Root Beer");
            collection[3] = new JellyBean("French Vanilla");
            
            // Create iterator
            JellyBeanIterator iterator = collection.CreateIterator();

            Console.WriteLine("Gimme all the jelly beans!");

            for (JellyBean item = iterator.First();
                !iterator.IsDone; item = iterator.Next())
            {
                Console.WriteLine(item.Flavor);
            }
        }

****************
Observer pattern
****************

**Observer pattern** allows one objects to notify other objects about changes in their state.

* **Subject** -> They are the publishers. When a change occurs to a subject it should notify all of its subscribers.

* **Observers** -> They are the subscribers. They simply listen to the changes in the subjects.

.. image:: images/ObserverPattern_1.png
   :width: 700

.. code-block:: c#
    :caption: Observer Pattern code example

        public interface IObserver
        {
            // Receive update from subject
            void Update(ISubject subject);
        }

        public interface ISubject
        {
            // Attach an observer to the subject.
            void Attach(IObserver observer);

            // Detach an observer from the subject.
            void Detach(IObserver observer);

            // Notify all observers about an event.
            void Notify();
        }

        // The Subject owns some important state and notifies observers when the
        // state changes.
        public class Subject : ISubject
        {
            // For the sake of simplicity, the Subject's state, essential to all
            // subscribers, is stored in this variable.
            public int State { get; set; } = -0;

            // List of subscribers. In real life, the list of subscribers can be
            // stored more comprehensively (categorized by event type, etc.).
            private List<IObserver> _observers = new List<IObserver>();

            // The subscription management methods.
            public void Attach(IObserver observer)
            {
                Console.WriteLine("Subject: Attached an observer.");
                this._observers.Add(observer);
            }

            public void Detach(IObserver observer)
            {
                this._observers.Remove(observer);
                Console.WriteLine("Subject: Detached an observer.");
            }

            // Trigger an update in each subscriber.
            public void Notify()
            {
                Console.WriteLine("Subject: Notifying observers...");

                foreach (var observer in _observers)
                {
                    observer.Update(this);
                }
            }

            // Usually, the subscription logic is only a fraction of what a Subject
            // can really do. Subjects commonly hold some important business logic,
            // that triggers a notification method whenever something important is
            // about to happen (or after it).
            public void SomeBusinessLogic()
            {
                Console.WriteLine("\nSubject: I'm doing something important.");
                this.State = new Random().Next(0, 10);

                Thread.Sleep(15);

                Console.WriteLine("Subject: My state has just changed to: " + this.State);
                this.Notify();
            }
        }

        // Concrete Observers react to the updates issued by the Subject they had
        // been attached to.
        class ConcreteObserverA : IObserver
        {
            public void Update(ISubject subject)
            {            
                if ((subject as Subject).State < 3)
                {
                    Console.WriteLine("ConcreteObserverA: Reacted to the event.");
                }
            }
        }

        class ConcreteObserverB : IObserver
        {
            public void Update(ISubject subject)
            {
                if ((subject as Subject).State == 0 || (subject as Subject).State >= 2)
                {
                    Console.WriteLine("ConcreteObserverB: Reacted to the event.");
                }
            }
        }
        
        class Program
        {
            static void Main(string[] args)
            {
                // The client code.
                var subject = new Subject();
                var observerA = new ConcreteObserverA();
                subject.Attach(observerA);

                var observerB = new ConcreteObserverB();
                subject.Attach(observerB);

                subject.SomeBusinessLogic();
                subject.SomeBusinessLogic();

                subject.Detach(observerB);

                subject.SomeBusinessLogic();
            }
        }

Reference : `Observer Pattern blog <https://www.exceptionnotfound.net/observer-pattern-in-csharp/>`_

*******************************
Chain Of Responsibility Pattern
*******************************

Avoid coupling the sender of a request to its receiver by giving more than one receiver object a chance to handle the request.

**Chain of responsibility** design pattern creates a chain of receiver objects for a given request.

Normally each receiver contains a reference to another receiver. If one receiver cannot handle the request then it passes the same request to the next receiver and so on.

.. image:: images/ChainOfResponsibility_Pattern_1.png
   :width: 700

.. image:: images/ChainOfResponsibility_Pattern_2.png
   :width: 700

.. code-block:: c#
    :caption: Chain of responsibility Pattern code example

        public abstract class Handler
        {
            public Handler rsHandler;
            public void nextHandler(Handler rsHandler)
            {
                this.rsHandler = rsHandler;
            }
            public abstract void dispatchRs(long requestedAmount);
        }

        public class TwoThousandHandler : Handler
        {
            public override void dispatchRs(long requestedAmount)
            {
                long numberofNotesToBeDispatched = requestedAmount / 2000;
                if (numberofNotesToBeDispatched > 0)
                {
                    if (numberofNotesToBeDispatched > 1)
                    {
                        Console.WriteLine(numberofNotesToBeDispatched + " Two Thousand notes are dispatched by TwoThousandHandler");
                    }                    
                }
                long pendingAmountToBeProcessed = requestedAmount % 2000;
                if (pendingAmountToBeProcessed > 0)
                {
                    rsHandler.dispatchRs(pendingAmountToBeProcessed);
                }
            }
        }

        public class FiveHundredHandler : Handler
        {
            public override void dispatchRs(long requestedAmount)
            {
                long numberofNotesToBeDispatched = requestedAmount / 500;
                if (numberofNotesToBeDispatched > 0)
                {
                    if (numberofNotesToBeDispatched > 1)
                    {
                        Console.WriteLine(numberofNotesToBeDispatched + " Five Hundred notes are dispatched by FiveHundredHandler");
                    }                    
                }
                long pendingAmountToBeProcessed = requestedAmount % 500;
                if (pendingAmountToBeProcessed > 0)
                {
                    rsHandler.dispatchRs(pendingAmountToBeProcessed);
                }
            }
        }

        public class ATM
        {
            private TwoThousandHandler twoThousandHandler = new TwoThousandHandler();
            private FiveHundredHandler fiveHundredHandler = new FiveHundredHandler();        
            
            public ATM()
            {
                // Prepare the chain of Handlers
                twoThousandHandler.nextHandler(fiveHundredHandler);
                
                //fiveHundredHandler.nextHandler(twoHundredHandler);               
            }
            public void withdraw(long requestedAmount)
            {
                twoThousandHandler.dispatchRs(requestedAmount);
            }
        }

        static void Main(string[] args)
        {
            ATM atm = new ATM();            
            atm.withdraw(4500);   // 2 * 2000 and 1 * 500
        }

*************
State Pattern
*************

**State Design Pattern** allows an object to alter its behavior when it’s internal state changes.

*Usage examples*: The State pattern is commonly used in C# to convert massive switch-base state machines into the objects.

The classes and objects participating in this pattern are:

**Context**

* defines the interface of interest to clients

* maintains an instance of a ConcreteState subclass that defines the current state.

**State**

* defines an interface for encapsulating the behavior associated with a particular state of the Context.

**Concrete States**

* each subclass implements a behavior associated with a state of Context

.. image:: images/StatePattern_1.png
   :width: 700

.. code-block:: c#
    :caption: State Pattern code example

        using System;

        namespace RefactoringGuru.DesignPatterns.State.Conceptual
        {
            // The Context defines the interface of interest to clients. It also
            // maintains a reference to an instance of a State subclass, which
            // represents the current state of the Context.
            class Context
            {
                // A reference to the current state of the Context.
                private State _state = null;

                public Context(State state)
                {
                    this.TransitionTo(state);
                }

                // The Context allows changing the State object at runtime.
                public void TransitionTo(State state)
                {
                    Console.WriteLine("Context: Transition to {state.GetType().Name}.");
                    this._state = state;
                    this._state.SetContext(this);
                }

                // The Context delegates part of its behavior to the current State
                // object.
                public void Request1()
                {
                    this._state.Handle1();
                }

                public void Request2()
                {
                    this._state.Handle2();
                }
            }
            
            // The base State class declares methods that all Concrete State should
            // implement and also provides a backreference to the Context object,
            // associated with the State. This backreference can be used by States to
            // transition the Context to another State.
            abstract class State
            {
                protected Context _context;

                public void SetContext(Context context)
                {
                    this._context = context;
                }

                public abstract void Handle1();

                public abstract void Handle2();
            }

            // Concrete States implement various behaviors, associated with a state of
            // the Context.
            class ConcreteStateA : State
            {
                public override void Handle1()
                {
                    Console.WriteLine("ConcreteStateA handles request1.");
                    Console.WriteLine("ConcreteStateA wants to change the state of the context.");
                    this._context.TransitionTo(new ConcreteStateB());
                }

                public override void Handle2()
                {
                    Console.WriteLine("ConcreteStateA handles request2.");
                }
            }

            class ConcreteStateB : State
            {
                public override void Handle1()
                {
                    Console.Write("ConcreteStateB handles request1.");
                }

                public override void Handle2()
                {
                    Console.WriteLine("ConcreteStateB handles request2.");
                    Console.WriteLine("ConcreteStateB wants to change the state of the context.");
                    this._context.TransitionTo(new ConcreteStateA());
                }
            }

            class Program
            {
                static void Main(string[] args)
                {
                    // The client code.
                    var context = new Context(new ConcreteStateA());
                    context.Request1();
                    context.Request2();
                }
            }
        }


****************
Template Pattern
****************

About Template Pattern