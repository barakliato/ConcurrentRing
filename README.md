# ConcurrentRing
C# thread safe implementation for ring as a replacement for a consumer producer queue

The class was built to support a connection buffer where one thread write from the connection and the other thread read from it.
The implementation provide an alternative to a consumer producer queue where it required a contiguous block of information without locks.
an exmaple of use:

ConcurrentRing<int> ring = new ConcurrentRing<int>(500);
Action a1 = () =>
{
    while (true)
    {
        ring.Write(new int[] { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 });
        Task.Delay(25).Wait();
    }
};

Action a2 = () =>
{
    while (true)
    {
        var items = ring.Read(10);
        foreach (var item in items)
            Console.Write("{0} ", item);

        Console.WriteLine();
        Task.Delay(10).Wait();
    }
};

var t1 = Task.Run(a1);
var t2 = Task.Run(a2);
Task.WaitAll(t1, t2);
