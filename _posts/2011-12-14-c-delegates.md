---
layout: post
title: "C# Delegates"
categories: []
tags: []
published: true
---

Delegates can be little confusing at first sight, but with a little bit of patience and practice the concept becomes clear.

Recently, I purchased and watched the Mastering C# series from <a title="tekpub" href="http://tekpub.com/">Tekpub </a>(by <a href="http://msmvps.com/blogs/jon_skeet/">Jon Skeet</a> ) and I can say that this is by far the fastest way of learning and understanding a lot of .NET and programming concepts.


Take a look at a few aspects related to delegates. (clojures, anonymus methods, lambda expressions)


{% highlight csharp %}
namespace Delegates
{
    //delegate type declaration
    public delegate void StringAction(string value);

    //generic delegate declaration
    public delegate void GenericAction&lt;in T&gt;(T value);

    internal class Program
    {
        private static void Main(string[] args)
        {
            SimpleDelegates();
            AdvancedDelegates();
            Clojures();

            Console.ReadKey();
        }

        private static void SimpleDelegates()
        {
            StringAction stringAction = Console.WriteLine;
            stringAction("StringAction Delegate");

            GenericAction&lt;string&gt; genericAction = Console.WriteLine;
            genericAction("Generic Delegate");
        }

        private static void AdvancedDelegates()
        {
            //public delegate void Action&lt;in T&gt;(T obj)
            //Encapsulates a method that has a single parameter and does not return a value.
            Action&lt;string&gt; actionFromMethodName = Console.WriteLine;
            actionFromMethodName("Action delegate from method name");

            Action&lt;string&gt; actionFromAnonymusMethod = delegate(string x) { Console.WriteLine(x); };
            actionFromAnonymusMethod("Action delegate from anonymus method");

            Action&lt;string&gt; actionFromLambda = (x) =&gt; Console.WriteLine(x);
            actionFromLambda("Action delegate from Lambda Expression");

            //public delegate TResult Func&lt;in T,out TResult&gt;(T arg)
            //Encapsulates a method that has one parameter and returns a value of the type specified by the TResult parameter.
            Func&lt;int, string&gt; func = (x) =&gt; String.Format("Func delegate with {0} argument {1} ", x.GetType(), x);
            Console.WriteLine(func(10));
        }

        private static void Clojures()
        {
            const string outerConstant = "This is clojure in use";

            //clojure and lambda expression
            Action delegateUsingClojure = () =&gt; Console.WriteLine(outerConstant);
            delegateUsingClojure();

            //clojure and anonymus mehtod
            Action&lt;DateTime&gt; anonymusMethodClojure =
                delegate(DateTime time) { Console.WriteLine(String.Format("{0}:{1}", time, outerConstant)); };
            anonymusMethodClojure(DateTime.Now);
        }
    }
}
{% endhighlight %}

To really understand the usage of delegates, check out Jon's series <a title="edulinq" href="http://msmvps.com/blogs/jon_skeet/archive/tags/Edulinq/default.aspx">EduLinq</a>, an implementation from scratch of LINQ.
