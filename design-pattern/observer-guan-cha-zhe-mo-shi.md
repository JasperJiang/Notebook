# Observer 观察者模式

被观察者

```java
public interface IObject
    {
 
        IList<IMonitor> ListMonitor { get; set; } //定义观察者集合，因为多个观察者观察一个对象，所以这里用集合
        string SubjectState { get; set; }        //被观察者的状态
     
 
        void AddMonitor(IMonitor monitor);  //添加一个观察者
 
        void RemoveMonitor(IMonitor monitor); //移除一个观察者
 
        void SendMessage(); //向所有观察者发送消息
 
    }
    public class Subject : IObject
    {
 
 
        private IList<IMonitor> listMonitor = new List<IMonitor>();
 
        public string SubjectState //被观察者的状态
        {
            get;set;
        }
 
 
        public IList<IMonitor> ListMonitor //实现具体的观察者列表属性
        {
            get { return listMonitor; }
            set { listMonitor = value; }
        }
 
        public void AddMonitor(IMonitor monitor)  //实现具体的添加观察者方法
        {
            listMonitor.Add(monitor);
        
        }
 
        public void RemoveMonitor(IMonitor monitor) //实现具体的移除观察者方法
        {
           
            listMonitor.Remove(monitor);
        }
 
        public void SendMessage()    //实现具体的发送消息方法
        {
            foreach (IMonitor m in listMonitor)  //发送给所有添加过的观察者，让观察者执行update方法以同步更新自身状态
            {
                m.Update();
            }
        }
    }
```

观察者

```java
public interface IMonitor //定义观察者接口
    {
        void Update(); 
    }
 
 
    public class Monitor : IMonitor   //实现具体观察者
    {
        private string monitorState="Stop!";    //观察者初始状态，会随着被观察者变化而变化
        private string name;            //观察者名称，用于标记不同观察者
        private IObject subject;        //被观察者对象
 
 
 
        public Monitor (IObject subject, string name)  //在构造观察者时，传入被观察者对象，以及标识该观察者名称
        {
            this.subject = subject;
            this.name = name;
            Console.WriteLine("我是观察者{0}，我的初始状态是{1}", name, monitorState);
 
 
        }
 
 
        public void Update()                           //当被观察者状态改变，观察者需要随之改变
        {
            monitorState = subject.SubjectState;
            Console.WriteLine("我是观察者{0}，我的状态是{1}", name, monitorState);
 
        }
    }
```

前端调用

```java
static void Main(string[] args)
        {
 
            IObject subject = new Subject();
            subject.AddMonitor(new Monitor(subject, "Monitor_1"));
            subject.AddMonitor(new Monitor(subject, "Monitor_2"));
            subject.AddMonitor(new Monitor(subject, "Monitor_3"));
 
            subject.SubjectState = "Start!";
            subject.SendMessage();
 
            Console.Read();
        }
    }
```

结果如下

```text
我是观察者Monitor_1，我的初始状态是Stop!
我是观察者Monitor_2，我的初始状态是Stop!
我是观察者Monitor_3，我的初始状态是Stop!
我是观察者Monitor_1，我的状态是Start!
我是观察者Monitor_2，我的状态是Start!
我是观察者Monitor_3，我的状态是Start!
```

