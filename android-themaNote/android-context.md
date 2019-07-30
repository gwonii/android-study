## Android Item1

### Context



#### 1. context란?

```t
Class Overview
Interface to global information about an application environment. This is an abstract class whose implementation is provided by the Android system. It allows access to application-specific resources and classes, as well as up-calls for application-level operations such as launching activities, broadcasting and receiving intents, etc.

어플리케이션 환경에 관한 글로벌 정보를 접근하기 위한 인터페이스. Abstract 클래스이며 실재 구현은 안드로이드 시스템에 의해 제공된다. Context 를 통해, 어플리케이션에 특화된 리소스나 클래스에 접근할 수 있을 뿐만 아니라, 추가적으로, 어플리케이션 레벨의 작업 - Activity 실행, Intent 브로드캐스팅, Intent 수신 등, 을 수행하기 위한 API 를 호출 할 수도 있다.
```

* 어플리케이션에 관하여 시스템이 관리하고 있는 정보에 접근하기 
* 안드로이드 시스템 서비스에서 제공하는 API를 호출할 수 있는 기능 

### 2. 왜 context가 필요한가? 

보통은 시스템 기능을 수행하기 위해, 시스템 함수를 호출하는 일은 안드로이드가 아닌 다른 플랫폼에서도 늘상 일어난다. 그리고 어떠한 매개체를 거칠 필요없이 직접적으로 시스템 API를 호출하면 된다. 

반면 안드로이드에서는 Context라는 인스턴스화된 매개체를 통해야만 유사한 일들을 수행할 수 있다.

**C#**

```c++
 //Get an Application Name.
 String applicationName = System.AppDomain.CurrentDomain.FriendlyName;

 //Start a new process(application)
 System.Diagnostics.Process.Start("test.exe");
```



**안드로이드 Activity**

```java
//Get an application name
String applicationName = this.getPackageName();

//Start a new activity(application)
this.startActivity(new Intent(this, Test.class));
```

> 위 처럼, C#의 경우 System단에서 제공하는 정적 함수를 호출 함으로써 간단하게 할 수 있는 일들을 안드로이드에서는 Context에 정의된 인스턴스 함수를 호출해야만 가능하게 되어있다. 즉, 위에서 처럼 반드시 인스턴스화된 Context 클래스를 사용해야 된다. 

### 3. 일반적인 OS와 안드로이드의 차이 

#### 3.1 OS 커널 

* 일반적인 OS 커널이 하는 일 중에 하나는 프로세스를 관리하는 것이다. 특정 프로세스가 특정 어플리케이션과 맵핑된다면, 우리는 별 다른 매개체 없이 시스템에 직접 프로세스의 정보에 관해서 물어볼 수 있고, 프로세스와 연관된 시스템 함수를 호출할 수 있다. 

#### 3.2 안드로이드 

* 그런데 안드로이드에서는 어플리케이션과 프로세스가 서로 독립적으로 구성되어 있다. 그렇기 때문에 Context라는 객체를 형성하여 context를 이용하여 시스템 처리를 하는 것이다. 

> 안드로이드도 프로세스는 OS 커널에서 관리된다. 

* 그렇다면 안드로이드에서 어플리케이션 정보는 어디서 관리가 되는 것인가? 

안드로이드의 시스템 서비스 중 하나인 ActivityMangerService에서 그 책임을 진다. ActivityMangerService에서 특정 토큰을 키값으로 "Key - Value" 쌍으로 이루어진 배열을 이용해 현재 작동중인 어플리케이션을 정보를 관리한다. 

#### 3.3 결론 

Context는 어플리케이션과 관련된 정보에 접근하고자 하거나 어플리케이션과 연관된 시스템 레벨의 함수를 호출하고자 할 때 사용된다. 그런데 안드로이드 시스템에서 어플리케이션 정보를 관리하고 있는 것은 시스템이 아닌, ActivityMangerService라는 일종의 또 다른 어플리케이션이다. 따라서 다른 일반적인 플랫폼과는 달리, 안드로이드에서는 어플리케이션과 관련된 정보에 접근하고자 할 때에는 ActivityMagnerService를 통해야만 한다. 



