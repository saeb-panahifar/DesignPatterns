
Visitor pattern is a part of behavioral desgin pattern.
Visitor pattern will not change the current code structure. It is used to add the new functionality in existing code.
Classes which holds the new behaviors are commonly known as Visitors.

Key Participants
Visitor           It declares a Visit operation for each class of ConcreteElement in the object structure. The operation's name and signature identifies the class.
ConcreteVisitor   It implements each operation declared by Visitor.
Element           It defines an Accept operation that takes a visitor as an argument.
ConcreteElement   It implements an Accept operation that takes a visitor as an argument.
ObjectStructure   It holds all the element of the data structure as a collection, list of something which can be enumerated and used by the visitors.



