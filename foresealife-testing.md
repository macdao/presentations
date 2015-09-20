# Testing


内容

- JUnit
- Mockito
- TDD
- 测试金字塔



## JUnit

- http://junit.org/
- 是一个Java单元测试框架
- A research survey performed in 2013 across 10,000 GitHub projects found that JUnit, along with slf4j-api, are the most popular libraries. Each library was used by 30.7% of projects.


### Create a test

```java
import org.junit.Test;

import static org.junit.Assert.assertTrue;

public class MyTest {
    @Test
    public void testNewArray() throws Exception {
        final boolean result = false;
        assertTrue(result);
    }
}
```


### Assertions

```
java.lang.AssertionError
	at org.junit.Assert.fail(Assert.java:86)
	at org.junit.Assert.assertTrue(Assert.java:41)
	at org.junit.Assert.assertTrue(Assert.java:52)
	at macdao.MyTest.testNewArray(MyTest.java:11)
```


### assertThat

```java
assertThat([value], [matcher statement]);
```

Note: Joe Walnes built a new assertion mechanism on top of what was then JMock 1


```java
import org.junit.Test;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

public class MyTest {
    @Test
    public void testNewArray() throws Exception {
        final boolean result = false;
        assertThat(result, is(true));
    }
}

```
```
Expected: is <true>
     but: was <false>
```


### matchers

- org.hamcrest.CoreMatchers

```java
assertThat("good", is("good"));

assertThat(new Object(), notNullValue());

assertThat(new Object(), not(sameInstance(new Object())));

assertThat("good", startsWith("goo"));

assertThat(Arrays.asList(1, 2), hasItem(1));
```


```java
assertThat("fab", both(containsString("a")).and(containsString("b")));

assertThat("good", not(allOf(equalTo("bad"), equalTo("good"))));

assertThat("good", anyOf(equalTo("bad"), equalTo("good")));

assertThat(7, not(either(equalTo(3)).or(equalTo(4))));
```


### Ignoring a Test

```java
@Ignore("Test is ignored as a demonstration")
@Test
public void testSame() {
    assertThat(1, is(1));
}
```


### fixture annotations

```java
import org.junit.*;
import static java.lang.System.out;
public class MyTest {
    @BeforeClass
    public static void setUpClass() { out.println("@BeforeClass"); }
    @AfterClass
    public static void tearDownClass() { out.println("@AfterClass"); }
    @Before
    public void setUp() throws Exception { out.println("@Before"); }
    @After
    public void tearDown() throws Exception { out.println("@After"); }
    @Test
    public void test1() throws Exception { out.println("test1"); }
    @Test
    public void test2() throws Exception { out.println("test2"); }
}
```
```
@BeforeClass
@Before
test1
@After
@Before
test2
@After
@AfterClass
```


### Expected Exceptions

```java
@Test(expected = IndexOutOfBoundsException.class)
public void empty() {
     new ArrayList<Object>().get(0);
}
```


```java
@Test
public void testExceptionMessage() {
    try {
        new ArrayList<Object>().get(0);
        fail("Expected an IndexOutOfBoundsException to be thrown");
    } catch (IndexOutOfBoundsException exception) {
        assertThat(exception.getMessage(), is("Index: 0, Size: 0"));
    }
}
```


```java
@Rule
public ExpectedException thrown = ExpectedException.none();

@Test
public void shouldTestExceptionMessage() throws IndexOutOfBoundsException {
    List<Object> list = new ArrayList<Object>();

    thrown.expect(IndexOutOfBoundsException.class);
    thrown.expectMessage("Index: 0, Size: 0");
    list.get(0); // execution will never get past this line
}
```


### System Rules

```java
public void MyTest {
	@Rule
	public final SystemOutRule systemOutRule = new SystemOutRule().enableLog();

	@Test
	public void writesTextToSystemOut() {
		System.out.print("hello world");
		assertEquals("hello world", systemOutRule.getLog());
	}
}
```



# Test Double


## 测试Student

反例

```java
@Test
public void student_1_should_say_1() throws Exception {
    final MultipleGameRule rule1 = new MultipleGameRule(fizzGame);
    final DefaultGameRule rule2 = new DefaultGameRule();
    final ArrayList<GameRule> rules = Lists.newArrayList(rule1, rule2);
    final Student student = new Student(rules, 1);

    assertThat(student.say(), is("1"));
}
```

Note: 同时把MultipleGameRule等其他类引入了该测试


## SUT

- System Under Test


## Student的测试

- Case 1
 - Given两个规则
 - When第一个规则返回了Fizz
 - Then返回Fizz
- Case 2
 - Given两个规则
 - When第一个规则没有返回值
 - 而第二个规则返回了Buzz
 - Then返回Buzz


## Case 1

```java
@Test
public void say_fizz_when_first_rule_return_fizz() throws Exception {
    final GameRule rule1 = new GameRule() {
        @Override
        public Optional<String> say(int index) {
            return Optional.of("Fizz");
        }
    };
    final GameRule rule2 = new GameRule() {
        @Override
        public Optional<String> say(int index) {
            return Optional.absent();
        }
    };
    final Student student = new Student(Lists.newArrayList(rule1, rule2), 1);
    assertThat(student.say(), is("Fizz"));
}
```


## Mockito

```java
import static org.mockito.Mockito.*;

List mockedList = mock(List.class);

when(mockedList.get(0)).thenReturn("first");
when(mockedList.get(1)).thenThrow(new RuntimeException());

assertThat(mockedList.get(0), is("first"));

```


```java
Car boringStubbedCar = when(mock(Car.class).shiftGear())
        .thenThrow(EngineNotStarted.class)
        .getMock();

doThrow(new RuntimeException()).when(mockedList).clear();
```


```java
List mockedList = mock(List.class);

mockedList.add("one");
mockedList.clear();

verify(mockedList).add("one");
verify(mockedList).clear();
```


## Argument matchers

```java
when(mockedList.get(anyInt())).thenReturn("element");

assertThat(mockedList.get(999), is("element"));

verify(mockedList).get(anyInt());
```


## Verifying number of invocations

```java
mockedList.add("twice");
mockedList.add("twice");

mockedList.add("three times");
mockedList.add("three times");
mockedList.add("three times");

verify(mockedList, times(2)).add("twice");

verify(mockedList, never()).add("never happened");
verify(mockedList, atLeastOnce()).add("three times");
verify(mockedList, atLeast(2)).add("five times");
verify(mockedList, atMost(5)).add("three times");
```



# Test-driven development (TDD)


## 目标：Clean code that works


## Overview

- a software development process
- 极限编程
- by Kent Beck


## 测试驱动开发的规则

- 只有在测试失败时，我们才写新的代码
- 消除重复


## 这意味着

- 用测试来驱动开发
- 一边写测试一边开发
- 开发环境可以对小改动快速响应
- 为方便测试，代码必须高内聚低耦合


## 步骤

<!-- .slide: data-background="white" -->

![TDD Circle of Life](huawei-tdd/tdd-circle-of-life.png)


# Behavior-driven development (BDD)

- a software development process
- developed by Dan North
- as a response to the issues encountered teaching TDD


- Test method names should be sentences
- A simple sentence template keeps test methods focused
- An expressive test name is helpful when a test fails
- `Behaviour` is a more useful word than `test`
- given-when-then template
 - Given some initial context (the givens)
 - When an event occurs
 - then ensure some outcomes


## 2 levels of TDD

- Acceptance TDD
- Developer TDD


![ATDD](huawei-tdd/atdd.jpg)



# 测试金字塔


<!-- .slide: data-background="white" -->

![pyramid](huawei-tdd/pyramid.png)

Note: UI tests is brittle, expensive to write, and time consuming to run. high-level tests are there as a second line of test defense. If you get a failure in a high level test, not just do you have a bug in your functional code, you also have a missing unit test.



# 参考资料

- [Mocks Aren't Stubs](http://martinfowler.com/articles/mocksArentStubs.html)
- [Mockito](http://site.mockito.org/mockito/docs/current/org/mockito/Mockito.html)
- [Test Driven Development: By Example](http://www.amazon.com/Test-Driven-Development-By-Example/dp/0321146530)
- [UnitTest](http://martinfowler.com/bliki/UnitTest.html)
- [Introduction to Test Driven Development](http://agiledata.org/essays/tdd.html)
- [Introducing BDD](http://dannorth.net/introducing-bdd/)
- [Stop mocking, start newing](http://mrcoder.github.io/stop-mocking-start-newing/)
- [Java refactoring features of IntelliJ IDEA](https://www.jetbrains.com/idea/features/refactoring.html)
