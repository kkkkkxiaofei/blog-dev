---
layout: post_layout
title:  "Mock & Stub"
date:   2016-02-01 23:09PM
categories: jekyll update
type: 2
summary: "Both mock and stub are mummy objects for unit test in spring.When you have lots of dependencies in unit test, creating fake object to reduce dependency is really recommended. Therefore, we use mock and stub. But there are some differences between mock and stub."
icon: "java-icon.jpg"
---
#### Abstract

>Both mock and stub are mummy objects for unit test in spring.When you have lots of dependencies in unit test, creating fake object to reduce dependency is really recommended. Therefore, we use mock and stub. But there are some differences between mock and stub.

#### Stub

Stub is a common way to use without extra dependency in unit test.It trys to describe the behevior of the method, So we just concern about the return value when use stub.

Here is example:

{% highlight java %}

class CashRegister {
    public void process(Purchase purchase, Printer printer) {
        printer.print(purchase.asString());
    }
}

{% endhighlight %}

Now we have a method in the instance of the CashRegister, It have purchase and printer so that It can print the bill when invoke process.

But it is not realistic for us to use a real printer in our test, so we try to use a fake printer to do unit test.

In stub approach:

We create a sub printer

{% highlight java %}

public class FakePrinter extends Printer {
    public boolean wasInvoked;
    @Override
    public void print(String printThis) {
        wasInvoked = true;
    }
}

{% endhighlight %}

We test "if printer is invoked when process"

{% highlight java %}

@Test
public void shouldPrintInfoOfPurchase() throws Exception {
    FakePrinter fakePrinter = new FakePrinter();
    Item[] items = {
        new Item("xiaofei", 200.00)
    };
    CashRegister cashRegister = new CashRegister();
    Purchase purchase = new Purchase(items);
    cashRegister.process(purchase, fakePrinter);
    assertTrue(fakePrinter.wasInvoked);
}

{% endhighlight %}

#### Mock

Mock is similar with stub, but mock is a real fake object.

We can test the above method as:

{% highlight java %}

@Test
public void shouldPrintInfoOfPurchaseWithMockPrinter() throws Exception {
    Printer fakePrinter = Mockito.mock(Printer.class);
    Item[] items = {
        new Item("xiaofei", 200.00)
    };
    CashRegister cashRegister = new CashRegister();
    Purchase purchase = new Purchase(items);
    cashRegister.process(purchase, fakePrinter);
    verify(fakePrinter).print(purchase.asString());
}

{% endhighlight %}

By using the framework of Mockito, we can create a fake object by Class, and the verify assertion can check if the method has been invoked.

#### Summary

According to [Martin Fowler's](http://martinfowler.com/articles/mocksArentStubs.html) article:

- `Dummy` objects are passed around but never actually used. Usually they are just used to fill parameter lists.

- `Fake` objects actually have working implementations, but usually take some shortcut which makes them not suitable for production (an in memory database is a good example).

- `Stubs` provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test. Stubs may also record information about calls, such as an email gateway stub that remembers the messages it 'sent', or maybe only how many messages it 'sent'.

- `Mocks` are what we are talking about here: objects pre-programmed with expectations which form a specification of the calls they are expected to receive.