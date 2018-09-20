
* Visitor pattern is a part of behavioral desgin pattern.

* Visitor pattern will not change the current code structure. It is used to add the new functionality in existing code.
Classes which holds the new behaviors are commonly known as Visitors.

# Key Participants
**Visitor**:         It declares a Visit operation for each class of ConcreteElement in the object structure. The operation's name and signature identifies the class.

**ConcreteVisitor**:   It implements each operation declared by Visitor.

**Element**:           It defines an Accept operation that takes a visitor as an argument.

**ConcreteElement**:   It implements an Accept operation that takes a visitor as an argument.

**ObjectStructure**:   It holds all the element of the data structure as a collection, list of something which can be enumerated and used by the visitors.



```c#
//Element
 public abstract class Food
 {
        public string Name { get; set; }
        public decimal Price { get; set; }
        public int Count { get; set; }

        public Food(string name, decimal price, int count)
        {
            Name = name;
            Price = price;
            Count = count;
        }

        public abstract void Accept(Visitor visitor);
 }
 
 //ConcreteElement
 public class Pizza : Food
 {
        public Pizza(string name, decimal price, int count)
            : base(name, price, count)
        {

        }
        public override void Accept(Visitor visitor)
        {
            visitor.Visit(this);
        }
}

//ConcreteElement
public class Pasta : Food
{
        public Pasta(string name, decimal price, int count)
            : base(name, price, count)
        {

        }
        public override void Accept(Visitor visitor)
        {
            visitor.Visit(this);
        }
}

//Visitor
public abstract class Visitor
{
        public abstract void Visit(Pasta pasta);
        public abstract void Visit(Pizza pizza);
}

//ConcreteVisitor
public class DiscountVisitor : Visitor
{
        public override void Visit(Pasta pasta)
        {
            var totalPrice = pasta.Price * pasta.Count;
            byte discount = 10;

            pasta.Price = totalPrice - (totalPrice * discount / 100);
        }

        public override void Visit(Pizza pizza)
        {
            byte discount = 0;

            if (pizza.Count < 5)
            {
                discount = 10;
            }
            else if (pizza.Count >= 5 && pizza.Count < 20)
            {
                discount = 20;
            }
            else
            {
                discount = 30;
            }

            var totalPrice = pizza.Price * pizza.Count;
            pizza.Price = totalPrice - (totalPrice * discount / 100);

        }
}

//ObjectStructure
public class OrderList : List<Food>
{
        public void Attach(Food element) => Add(element);

        public void Dettach(Food element) => Remove(element);

        public void Accept(Visitor visitor) => this.ForEach(x => x.Accept(visitor));

        public void PrintBill() => this.ForEach(x => Print(x));

        private void Print(Food food)
        {
            Console.WriteLine(string.Format("FoodName: {0}, Price:{1}, Count:{2}", food.Name, food.Price, food.Count));
        }

}


class Program
{
        static void Main(string[] args)
        {
            OrderList orders = new OrderList();
            orders.Add(new Pizza("Neapolitan Pizza", 45000, 2));
            orders.Add(new Pasta("Spaghetti ", 30000, 1));

            DiscountVisitor visitor = new DiscountVisitor();
            orders.Accept(visitor);

            orders.PrintBill();

            Console.ReadLine();
        }
}
```
