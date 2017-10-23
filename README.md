# PowerMock-and-Mockito-Learning
## 使用实例
### 1. 模拟接口返回
    InterfaceToMock mock = PowerMockito.mock(InterfaceToMock.class);
    Powermockito.when(mock.method(Params...)).thenReturn(value);
    Powermockito.when(mock.method(Params...)).thenThrow(Exception);
## 2.设置对象private属性
    使用 whitebox 向 class 或者对象中赋值 
    Whitebox.setInternalState(Object object, String fieldname, Object... value)
    object 为需要设置属性的静态类或者对象    
### 3.模拟构造函数
    加入注释
    @RunWith(PowerMockRunner.class)
    @PrepareForTest({InstanceClass.class})
    @PowerMockIgnore("javax.management.*")
    Powermockito.whenNew(InstanceClass.class).thenReturn(Object value)
### 4.模拟静态方法 
    类似于模拟构造函数 加入注释
    @RunWith(PowerMockRunner.class)
    @PrepareForTest({StaticClassToMock.class})
    @PowerMockIgnore("javax.management.*")
    Powermockito.mockStatic(StaticClassToMock.class);
    Powermockito.when(StaticClassToMock.method(Object... params)).thenReturn(Object value)
### 5.模拟final方法
    Final方法的模拟类似于模拟静态方法
    @RunWith(PowerMockRunner.class)
    @PrepareForTest({FinalClassToMock.class})模拟
    @PowerMockIgnore("javax.management.*")
    Powermockito.mockStatic(FinalClassToMock.class);
    Powermockito.when(StaticClassToMock.method(Object... params)).thenReturn(Object value)
### 6.模拟静态类
    类似于模拟静态方法
### 7.使用spy方法避免执行被测类中的成员函数
    例如 被测试类为： TargetClass,想要屏蔽的方法为targetMethod.
    1) PowerMockito.spy(TargetClass.class);
    2) Powermockito.when(TargetClass.targetMethod()).doReturn()
    3) 加入注释
        @RunWith(PowerMockRunner.class)
        @PrepareForTest(DisplayMoRelationBuilder.class)
        @PowerMockIgnore("javax.management.*")
### 8.参数匹配器
    Mockito.anyInt(), Mockito.anyString()
### 9.处理public void 类型的静态方法
    Powermockito.doNothing.when();
    
    
# 如果想使用@Mock, @Spy, @InjectMocks等注解时，需要进行初始化后才能使用。
## 初始化的方法有3种：
1，在Junit的类上面使用@RunWith(MockitoJUnitRunner.class)注解。
    但如果你使用的是Spring的话，可能你会使用Spring的测试类（@RunWith(SpringJUnit4ClassRunner.class)）
    这样的话，你就没有办法使用上面的@RunWith(SpringJUnit4ClassRunner.class)。你还可以使用下面的方法。

2，在测试方法被调用之前，使用MockitoAnnotations.initMocks(this);。例如：
    @Before
    public void initMocks(){
        MockitoAnnotations.initMocks(this);
    }
    @Before保证了在被测试的方法被调用之前，调用@Before所注解的方法
    这个方法还有下面的好处。（具体是什么样子还没有试过）
        Allows shorthand creation of objects required for testing.
        Minimizes repetitive mock creation code.
        Makes the test class more readable.
        Makes the verification error easier to read because field name is used to identify the mock.

3，使用MockitoRule。如下：
        @Rule
        public MockitoRule rule = MockitoJUnit.rule();
        这个方法会调用validateMockitoUsage方法。这个方法有一些好处，具体什么好处还不太清楚。
    
# mock Exception
    PowerMockito.when(objectMapper.readValue(TEST_STRING1, Map.class)).thenThrow(IOException.class);
# 模拟private函数和局部模拟
    参考https://www.ibm.com/developerworks/cn/java/j-lo-powermock/
## 模拟私有以及 Final 方法
   局部模拟则提供了另外一种方式，在使用局部模拟时，被创建出来的模拟对象依然是原系统对象，虽然可以使用方法 When().thenReturn()来指定某些具体方法的  返回值，但是没有被用此函数修改过的函数依然按照系统原始类的方式来执行。这种局部模拟的方式的强大之处在于，除开一般方法可以使用之外，Final 方法和私有方法一样可以使用。
### 被测试代码
    public final class PrivatePartialMockingExample { 
        public String methodToTest() { 
            return methodToMock("input"); 
        } 
        private String methodToMock(String input) { 
            return "REAL VALUE = " + input; 
        } 
    }
### 局部模拟方法实现
    @RunWith(PowerMockRunner.class) 
    @PrepareForTest(PrivatePartialMockingExample.class) 
    public class PrivatePartialMockingExampleTest { 
    @Test 
        public void demoPrivateMethodMocking() throws Exception { 
            final String expected = "TEST VALUE"; 
            final String nameOfMethodToMock = "methodToMock"; 
            final String input = "input"; 
            PrivatePartialMockingExample underTest = spy(new PrivatePartialMockingExample()); 
 
            /* 
            * Setup the expectation to the private method using the method name 
            */ 
            when(underTest, nameOfMethodToMock, input).thenReturn(expected); 

            assertEquals(expected, underTest.methodToTest()); 

            // Optionally verify that the private method was actually called 
            verifyPrivate(underTest).invoke(nameOfMethodToMock, input); 
       } 
    }
   可以发现，为了实现局部模拟操作，用来创建模拟对象的函数从 mock() 变成了 spy()，操作对象也从类本身变成了一个具体的对象。同时，When() 函数也使用了不同的版本：在模拟私有方法或者是 Final 方法时，When() 函数需要依次指定模拟对象、被指定的函数名字以及针对该函数的输入参数列表。
    powermock 官方文档 https://github.com/powermock/powermock
    mockito 官方文档 https://github.com/mockito/mockito
