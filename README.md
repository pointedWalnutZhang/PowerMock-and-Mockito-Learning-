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
    
        
        
        
