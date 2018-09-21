
- Template Method lets subclasses redefine certain steps of an algorithm
without changing the Algorithm's Structure. 

**Example in the .Net framework**:

The most common example is the Asp.Net Page Life Cycle. The Page Life Cycle has a few methods which are called in a sequence but we have the liberty to modify the functionality of any of the methods by overriding them.


## Key Participants

**AbstractClass**:  Defines abstract primitive operations that concrete subclasses define to implement steps of an algorithm.

**ConcreteClass**:  Implements the primitive operations ot carry out subclass-specific steps of the algorithm.

```c#
public enum Priority
{
        Hight = 1,
        Medium = 2,
        Low = 3
}

public interface ITask
{
        void Start();
}

//AbstractClass
public abstract class Task : ITask
{
        public abstract Priority Priority { get; }

        void ITask.Start()
        {
            if (HasAccess())
            {
                OnStart();
            }
        }

        protected bool HasAccess()
        {
            //if user access to run this task true, otherwise false.
            return true;
        }

        protected abstract void OnStart();

}

//ConcreteClass
public class ATask : Task
{
        public override Priority Priority => Priority.Hight;

        protected override void OnStart()
        {
            Console.WriteLine("ATask Run.");
        }
}

//ConcreteClass
public class BTask : Task
{
        public override Priority Priority => Priority.Low;

        protected override void OnStart()
        {
            Console.WriteLine("BTask Run.");
        }
}

public sealed class TaskRunner
{
        private readonly List<Task> tasks = new List<Task>();

        public TaskRunner()
        {
        }

        public void Run()
        {
            RegisterAllTaskSubclass();

            foreach (ITask item in tasks.OrderBy(a => a.Priority.GetHashCode()))
            {
                item.Start();
            }
        }

        private void RegisterAllTaskSubclass()
        {
            var taskType = AppDomain.CurrentDomain.GetAssemblies()
                                  .SelectMany(assembly => assembly.GetTypes())
                                  .Where(type => type.IsSubclassOf(typeof(Task)));

            foreach (Type item in taskType)
            {
                tasks.Add((Task)Activator.CreateInstance(item));
            }
        }
}
    
class Program
{
        static void Main(string[] args)
        {

            var taskRunner = new TaskRunner();
            taskRunner.Run();

            Console.ReadLine();
        }
}
    
```
