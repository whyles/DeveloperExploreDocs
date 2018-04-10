# Fragment

  Fragment表示activity中的行为或用户界面的一部分。你可以将多个Fragment组合在一个activity中来构建多窗格UI以及在多个activity中重复使用某个fragmen。你可以认为Fragment是activity的模块化组成部分，它有自己的生命周期，可以接收自己的输入事件，并且你可以在activity运行时添加或移除Fragment。可以认为它是能在不同activity中重复使用的子Activity。
  
  Fragment必须始终嵌入activity中，它的生命周期受宿主activity生命周期的影响。当activity暂停时，其中所有的Fragemnt也会暂停；当activity销毁时，其中所有的fragment也会销毁。不过当activity处于正在运行时，你可以独立操作每个Fragment，如添加或移除它们。当执行此类Fragment事务时，您也可以将其添加到由 Activity管理的返回栈—Activity中的每个返回栈条目都是一条已发生Fragment事务的记录。 返回栈让用户可以通过按返回按钮撤消Fragment事务（后退）。
  
  当您将Fragment作为Activity布局的一部分添加时，它存在于Activity视图层次结构的某个ViewGroup 内部，并且Fragment会定义其自己的视图布局。您可以通过在Activity的布局文件中声明fragment，将其作为<fragment> 元素插入您的Activity布局中，或者通过将其添加到某个现有ViewGroup，利用应用代码进行插入。不过，fragment并非必须成为Activity布局的一部分；您还可以将没有自己UI的fragment用作 Activity 的不可见工作线程。
  
  ## 设计原理
  
  Android 在 Android 3.0（API级别11）中引入了fragment，主要是为了给大屏幕（如平板电脑）上更加动态和灵活的 UI 设计提供支持。由于平板电脑的屏幕比手机屏幕大得多，因此可用于组合和交换 UI 组件的空间更大。利用fragment实现此类设计时，您无需管理对视图层次结构的复杂更改。通过将 Activity 布局分成fragment，您可以在运行时修改Activity的外观，并在由Activity管理的返回栈中保留这些更改。同时也可以通过代码逻辑控制fragment的显示与否来适配平板电脑和手机屏幕差别较大的需求。
  
  ## 创建Fragment
  
  要想创建Fragment，你必须创建Fragment的子类（或已有其子类）。而且，通常你至少实现以下生命周期方法。
  
-   onCreate()
  
    系统会在创建fragment时调用此方法。您应该在其中初始化您想在fragment暂停或停止后恢复时保留的必需fragment组件。

-   onCreateView()
   
    系统会在fragment首次绘制其用户界面时调用此方法。要想为您的fragment绘制UI，您从此方法中返回的 View 必须是fragment布局的根视图。如果fragment未提供 UI，您可以返回 null。

-   onPause()
   
    系统将此方法作为用户离开fragment的第一个信号（但并不总是意味着此fragment会被销毁）进行调用。 您通常应该在此方法内确认在当前用户会话结束后仍然有效的任何更改（因为用户可能不会返回）。

![image](https://developer.android.com/images/fragment_lifecycle.png)


也可以扩展Fragment的已有子类来创建Fragment。

-   DialogFragment

    显示浮动对话框。使用此类创建对话框可有效地替代使用Activity类中的对话框帮助程序方法，因为您可以将Fragment对话框纳入由Activity管理的片段返回栈，从而使用户能够返回清除的fragment。
    
-  ListFragment

   显示由适配器（如 SimpleCursorAdapter）管理的一系列项目，类似于ListActivity。它提供了几种管理列表视图的方法，如用于处理点击事件的 onListItemClick() 回调。
   
-  PreferenceFragment
  
   以列表形式显示 Preference对象的层次结构，类似于PreferenceActivity。这在为您的应用创建“设置” Activity 时很有用处。

## 添加Fragment到Activity

  fragment一般都是被添加到activity中作为向用户展示界面的一部分。
  
### xml中静态添加fragment

        <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context="com.whyles.explore.fragmentsamples.activity.BasicsActivity">
    
        <fragment
            android:name="com.whyles.explore.fragmentsamples.fragment.BasicsFragment"
            android:id="@+id/fragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent">
            
        </fragment>
        
    </RelativeLayout>
    
<fragment> 中的 android:name 属性指定要在布局中实例化的 Fragment 类。

当系统创建此 Activity 布局时，会实例化在布局中指定的每个片段，并为每个片段调用onCreateView() 方法，以检索每个片段的布局。系统会直接插入片段返回的 View 来替代 <fragment> 元素。也就是说Fragment里的布局文件的根布局最终会被添加到activity布局中替代<fragment>的位置，而且<fragment>的id属性也会赋值在Fragment里的布局文件的根布局上面。

每个片段都需要一个唯一的标识符，重启Activity时，系统可以使用该标识符来恢复片段（您也可以使用该标识符来捕获片段以执行某些事务，如将其移除）。 可以通过三种方式为片段提供 ID：

- 为 android:id 属性提供唯一 ID。
- 为 android:tag 属性提供唯一字符串。
- 如果您未给以上两个属性提供值，系统会使用容器视图的ID。也就是说给<fragment>提供一个有id的容器视图也可以。

如果没有通过上面三种方式来为<fragment>设置唯一的标识符，系统在解析activity视图文件的时候会报错。

### 代码中动态添加fragment

代码中添加fragment主要通过FragmentTransaction类来执行fragment事物。而FragmentTransaction又是通过FragmentManager类来管理的。

    // 获取FragmentManager以及FragmentTransaction
    FragmentManager fragmentManager = getFragmentManager();
    FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
  
    // 添加事物到FragmentManager队列中
    ExampleFragment fragment = new ExampleFragment();
    fragmentTransaction.add(R.id.fragment_container, fragment);
    
    // 执行事物
    fragmentTransaction.commit();

### 添加没有UI的fragment

Fragment也可以为activity提供后台行为，而不显示UI.

## 管理Fragment

 管理Fragment需要使用到FragmentManager。FragmentManager主要与activity中fragment交互。
 
 - 获取FragmentManager
 
 调用getFragmentManager()获取FragmentManager对象。

- 获取Fragment

通过findFragmentById()和findFragmentByTag()获取Fragment。

- 获取FragmentTranscation

getFragmentManager().beginFragmentTranscation()。

- 操作返回栈

通过popBackStack()弹出返回栈。通过addOnBackStackChangedListener()注册一个侦听返回栈变化的侦听器。
 
## Fragment生命周期与事物

### Fragment生命周期

Fragment生命周期与Activity生命周期很相似。

     /**
     * fragment 绑定到 activity 时调用，只有执行了此方法切未执行unDetach()前，
     * Fragment的getActivity()才能正确获取到Activity实例。
     *
     * 可执行监听器注册和宿主activity实例获取等操作
     *
     * @param context
     */
    @Override
    public void onAttach(Context context) {
        super.onAttach(context);

        mActivity = (BasicsActivity) context;

        if (context instanceof OnFragmentInteractionListener) {
            mListener = (OnFragmentInteractionListener) context;
        } else {
            throw new RuntimeException(context.toString()
                    + " must implement OnFragmentInteractionListener");
        }
    }

    /**
     * 通过getArguments()获取Activity里面或者工厂方法获取实例里面通过setArguments()设置的参数。
     *
     * @param savedInstanceState
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (getArguments() != null) {
            mParam1 = getArguments().getString(ARG_PARAM1);
            mParam2 = getArguments().getString(ARG_PARAM2);
        }
    }

    /**
     * Fragment XML视图解析
     *
     * @param inflater
     * @param container
     * @param savedInstanceState
     * @return
     */
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        // 布局文件系统已经处理过会添加到activity的相应容器下面，故此处的attachToRoot选择false.
        return inflater.inflate(R.layout.fragment_lifecycle, container, false);
    }

    @Override
    public void onActivityCreated(@Nullable Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);
    }

    @Override
    public void onStart() {
        super.onStart();
    }

    @Override
    public void onResume() {
        super.onResume();
    }

    @Override
    public void onPause() {
        super.onPause();
    }

    @Override
    public void onStop() {
        super.onStop();
    }

    @Override
    public void onDestroyView() {
        super.onDestroyView();
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
    }

    /**
     * 解绑监听器，释放资源等。
     */
    @Override
    public void onDetach() {
        super.onDetach();
        mListener = null;
    }

但是Fragment所在的Activity生命周期会直接影响该Fragment的生命周期。Activity的每次生命周期回调都会引发其中fragment的类似生命周期的回调。只有Activity处于恢复状态时（运行时），fragment的生命周期才能独立变化。

下图是activity与fragment生命周期对应图：


![image](https://developer.android.com/images/activity_fragment_lifecycle.png)

### Fragment事物

Fragment事物提供了执行一组Fragment操作的方法，通过事物，我们可以在Activity中执行添加，移除，替换Fragment等操作。



    if (basicsFragment == null){
                basicsFragment = BasicsFragment.newInstance("", "");
            }
    
    FragmentTransaction fragmentTransaction = getFragmentManager().beginTransaction();
    fragmentTransaction.add(R.id.container, basicsFragment);
            
    fragmentTransaction.commit();
    
- 事物回退栈

fragment每起事物都需要最后提交的时候才能生效，而每起我们可以看做是一次操作，通过调用`addToBackStack()`可以将事物添加的返回栈中，当按返回按钮时系统会撤销此次操作。

    fragmentTransaction.addToBackStack(null);
    
- 事物的异步性

事物操作的执行是异步的，当调用add,replace,remove等操作时，是将这一系列操作加入FragmentMagager管理的操作队列中，并没有执行，当fragmentTransaction.commit()执行时才会正式执行。

### Fragment事物与生命周期与视图添加的关系

之前看过的fragment与activity生命周期对应图，但是那个只是一个正常流程情况下的对应图，也就是说系统帮我们默认做了添加Fragment到activity里面操作之后的生命周期图。

也就是说,在activity的运行前提下， 

- FragmentTransaction.add() 会执行Fragment的：
  
  onAttach(), onCreate(), onCreateView, onActivityCreate(), onStart(), onResume()。同时这时候也会将Fragment里面的根视图添加到activity的相应容器里面。
  
- FragmentTransaction.remove() 会执行Fragment的：
  
  onPause(), onStop(), onDestoryView(), onDestory(), onDtach()。同时Fragment根视图也会从Activity里相应的容器里面移除。

- FragmentTransaction.replace()

replace()里面本质上是先remove(),然后再add(),所以先是前一个Fragment执行remove()时的周期方法，再是后一个Fragment执行add()时的周期方法。这个流程在fragment事物操作未加入返回栈时是正确的，但当事物操作加入返回栈之后，将不会再执行remove()操作，只会执行add操作（）。

在未添加事物操作到返回栈时，replace()操作会从activity里移除之前的Fragment里面的根布局，添加后面一个Fragment视图的跟布局。但是如果Activity里面的那个fragment容器里面添加了多个fragment，由于FragmentManager管理的Fragment队列是共享一个Id的，而id又是只能对一个视图生效，故而当activity里面fragment容器已经添加了多个fragment的时候执行remove（）会将fragment队列里的第一个fragment移除，同时新添加的fragment加入的队列的末尾处。

但是当fragment事物添加到返回栈后，只会将新添加的fargment添加到FragmentManager里fragment队列的末尾。
    






## Activity 与 Fragment的交互

### Activity 与 Fragment的通信

尽管 Fragment 是作为独立于Activity的对象实现，并且可在多个Activity内使用，但Fragment的实例会直接绑定到包含它的 Activity。

所以，Fragment可以通过 getActivity() 访问 Activity 实例，并轻松地执行在 Activity 布局中查找视图等任务。

    View listView = getActivity().findViewById(R.id.list);
    
同样地，您的 Activity 也可以使用 findFragmentById() 或 findFragmentByTag()，通过从 FragmentManager 获取对 Fragment 的引用来调用片段中的方法。例如：

    ExampleFragment fragment = (ExampleFragment) getFragmentManager().findFragmentById(R.id.example_fragment);

### 创建事件回调

## 支持库中的Fragment

## Fragment的"坑"

## Fragment的应用

## 实例
