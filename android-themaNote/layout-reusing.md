# xml 재사용하기 



## 1. xml 정의하기 

* 기본적인 View를 구성할 때, 반복적으로 사용하게 되는 뷰가 있다면 ViewGroup을 만들어서 필요한 xml파일을 불러와서 재사용할 수 있다. 
* 재사용을 위해 include / merge를 사용할 수 있다. 

## 2. include와 merge의 사용 

* 기본적으로 include와 merge 태그는 View의 상속을 받는다 . 

```java
/* MainActivity */ 

View includeLayout = findViewById(R.id.in_include_layout);
```

> 이런 식으로 include와 merge의 id를 이용하여 사용할 수 있다. 

### 2.1 include 

* layout에 만들어진 xml파일을 통해 view의 재활용을 가능하게 해준다.

**activity_main.xml** 

```xml
/* activity_main.xml */
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

<TextView 
	android:id="@+id=tv_title"
	android:layout_width="wrap_contents"
    android:layout_hegiht="wrap_contents"
    android:text="@string/title"/>

<include 
	android:id="@+id/include1"
	layout="@layout/custom_timeset"/>
	
        
</LinearLayout>
```

**custom_timeset.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    >
        
    <TextView
        android:id="@+id/tv_sub_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="지금은 공부를 시작할 시간입니다." />

    <Button
        android:id="@+id/bt_sub_title_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/tv_title"
        android:layout_alignEnd="@+id/tv_title"
        android:layout_alignParentStart="true"
        android:layout_marginStart="54dp"
        android:layout_marginTop="0dp"
        android:layout_marginEnd="52dp"
        android:layout_marginBottom="40dp"
        android:text="Button" />

</RelativeLayout>
```

> 1. 이런 식으로 만들어 놓은 xml 파일들을 재활용하여 view를 표현할 수 있다. 
> 2. merge만 사용할 때와 달리 include를 사용할 경우 id를 손쉽게 관리할 수 있다. 

### 2.2  ViewStub

* ViewStub은 View의 상속을 받는 크기가 0짜리인 더미 view이다. 
* 반드시 inflate를 통해 구체화시켜주어야 한다.
* 반드시 width와 height를 정의해주야 한다. 

**ativity_main.xml** 

```xml
/* include를 ViewStub으로만 반경 */

<ViewStub
	android:id="@+id/vs_viewStub1"
	android:layout_width="wrap_contents"
	android:layout_height="wrap_contens"
	android:layout="@layout/custom_timeset"//>
```

> 윗에서 말했듯이 width와 height를 정의해주어야 한다. 

**MainActivity.class**

```java
void onCreate() {
    ViewStub viewStub = findViewById(R.id.vs_viewStub1);
    View inflated = viewStub.inflate();
    
    TextView subTitle = inflated.findeViewById(R.id.tv_sub_title);
    subTitle.setText("subTitle 변경");
}
```

> 위 처럼 java에서 inflate를 해서 객체화한 후에 내부의 id에 접근할 수 있다. 

### 2.3 merge

* 단순히 위의 예시에서 RelativeLayout을 merge로 바꾼 것이다. 

* merge 태그를 이용한 xml 파일을 사용하려고 할 때, merge Layer가 무시되고 안에 들어있는 view들이 바로 인식된다.  그렇기 때문에, 단순히 include 한 것보다 depth가 낮다고 할 수 있다. 

**main_activity.xml**

```xml
/* activity_main.xml */
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

<TextView 
	android:id="@+id=tv_title"
	android:layout_width="wrap_contents"
    android:layout_hegiht="wrap_contents"
    android:text="@string/title"/>

<include 
	android:id="@+id/include1"
	layout="@layout/custom_timeset"/>
	
</LinearLayout>
```



**custom_timeset.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge>
        
    <TextView
        android:id="@+id/tv_sub_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="지금은 공부를 시작할 시간입니다." />

    <Button
        android:id="@+id/bt_sub_title_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/tv_title"
        android:layout_alignEnd="@+id/tv_title"
        android:layout_alignParentStart="true"
        android:layout_marginStart="54dp"
        android:layout_marginTop="0dp"
        android:layout_marginEnd="52dp"
        android:layout_marginBottom="40dp"
        android:text="Button" />

</merge>
```

