# LeanTest
A shared library used to simplify the process of creating and maintaining simple and consistent developer tests across projects.

The concept of _developer testing_ is not very well understood. This is my fault, but watch [my blog](https://blog.elgaard.com), I intend to eventually explain the concept in great detail.

In short developer testing is about developers writing tests while developing code. And I mean tests that testers would call _real_ tests, not simply unit tests. Tests which are simple to write initially, then simple to maintain going forward. Tests which cover actual functionality which is recognisable and valuable to the business.

## An Example
Note that the test below declares all _data_ that it depends on in a `WithData` method. This is quite central to lean tests - the target under test is always _empty_ with respect to data before each test, while any data that each individual test depends on is explicitly declared. In this case, the data is fed into a simple mock of an external service. In other cases the data could be fed into an (initially empty) database or other means for keeping state.

````csharp
        [TestMethod]
        public void GetAgeMustReturn10WhenKeyMatchesNewedUpData()
        {
            _contextBuilder
                .WithData(new MyData { Age = 10, Key = "ac_32_576259321" })
                .Build();

            int actual = _target.GetAge("FourtyTwo");

            Assert.AreEqual(10, actual);
        }
````

## References
An early version of LeanTest, as well as the concept of developer testing, was used extensively while developing Saxo Bank's social web site, [TradingFloor.com](https://www.tradingfloor.com/). Around 850+ developer tests (today 1000+ tests) covered most business functionality, running on a typical developer PC in around one minute. Major refactorings of the code were done with these tests as a safety net.

LeanTest is currently used internally in Saxo Bank's REST based [Open API](https://developer.saxo). I intend to also use LeanTest in my experimental [C# Open API](https://github.com/belgaard/TopOA) which wraps the official REST Open API.

