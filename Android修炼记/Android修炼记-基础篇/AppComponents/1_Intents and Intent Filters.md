# Intent

## Intent介绍

Intent是Android应用组件之间通信的消息体，它通常表明了两个通信组件的身份以及通信的内容。它最基础的3个用法如下：

- **Starting an activity**
  
  activity表示应用程序中的单个屏幕。您可以通过将Intent传递给startActivity（）来启动activity的新实例。Intent描述启动的activity并携带任何必要的数据。

  如果要在activity结束后收到返回结果，请调用startActivityForResult（）。您的activity能在onActivityResult（）回调中的单独Intent对象中接收到返回结果。
 

- **Starting a service**
  
  service是一个没有用户界面且在后台执行操作的应用组件。您可以通过将Intent传递给startService（）启动service来执行一次性操作（例如下载文件）。Intent描述启动的service并携带任何必要的数据。

  如果service是使用客户端-服务器接口设计的，则可以通过将Intent传递给bindService（）来启动绑定到另一个组件的服务。

- **Delivering a broadcast**

  Broadcast是任何应用可以接收的消息。系统为系统事件提供各种broadcast，例如系统启动或设备开始充电时。通过将Intent传递给sendBroadcast（）或sendOrderedBroadcast（）），您可以向其他应用程序发送Broadcast。
  
## Intent类型

Intent有两种类型：

- **Explicit intents（明确的意图；显式意图）**

  Explicit intents 通过名称（完全限定类名称）来指定要启动的组件。在应用中，一般我们都会通过Explicit intents来启动组件，因为一般我们都知道要启动的activity或者service的名称。
  
  当通过显式意图启动activity或service时，系统将立即启动intent中指定的应用程序组件。
  
- **Implicit intents（含蓄的意图；隐式意图）**

  Implicit intents 通过描述要启动组件的特点（如action，data等）来匹配相对应的组件，我们可以通过 Implicit intents访问其他应用程序或者系统声明的组件。
  
  当通过隐式意图启动activity或service时，Android系统通过将Intent的内容与在设备上包括其他应用的清单文件中声明的 Intent过滤器进行比较，从而找到要启动的相应组件。如果Intent与Intent过滤器匹配，则系统将启动该组件，并向其传递Intent对象。如果多个Intent过滤器兼容，则系统会显示一个对话框，支持用户选取要使用的应用。
  
  **注意**：为了确保应用的安全性，启动 Service 时，请使用显式 Intent，且不要为服务声明 Intent 过滤器。使用隐式 Intent启动服务存在安全隐患，因为您无法确定哪些服务将响应Intent，且用户无法看到哪些服务已启动。从Android5.0（API级别21）开始，如果使用隐式Intent调用bindService()，系统会引发异常。
  
## 构建Intent

Intent 对象携带了 Android 系统用来确定要启动哪个组件的信息（例如，准确的组件名称或应当接收该 Intent 的组件类别），以及收件人组件为了正确执行操作而使用的信息（例如，要采取的操作以及要处理的数据）。

### Intent中的主要信息


- **组件名称**

  要启动的组件名称。
  
  这是可选项，但也是构建显式Intent的一项重要信息，这意味着Intent应当仅传递给由组件名称定义的应用组件。 如果没有组件名称，则Intent是隐式的，且系统将根据其他Intent信息（例如，以下所述的操作、数据和类别）决定哪个组件应当接收Intent。因此，如需在应用中启动特定的组件，则应指定该组件的名称。

  注：启动 Service 时，您应始终指定组件名称。 否则，您无法确定哪项服务会响应 Intent，且用户无法看到哪项服务已启动。

  Intent 的这一字段是一个ComponentName对象，您可以使用目标组件的完全限定类名指定此对象，其中包括应用的软件包名称。 例如，com.example.ExampleActivity。您可以使用setComponent()、setClass()、setClassName() 或 Intent 构造函数设置组件名称。
  
- **action**

  指定要执行的通用操作（例如，“查看”或“选取”）的字符串。
  
  对于广播 Intent，这是指已发生且正在报告的操作。操作在很大程度上决定了其余Inten的构成，特别是数据和 extra 中包含的内容。

  您可以指定自己的操作，供Intent在您的应用内使用（或者供其他应用在您的应用中调用组件）。但是，您通常应该使用由 Intent 类或其他框架类定义的操作常量。以下是一些用于启动Activity的常见操作：

  - **ACTION_VIEW**
  
    如果您拥有一些某项 Activity可向用户显示的信息（例如，要使用图库应用查看的照片；或者要使用地图应用查看的地址），请使用 Intent 将此操作与 startActivity() 结合使用。
  
  - **ACTION_SEND**
  
    这也称为“共享”Intent。如果您拥有一些用户可通过其他应用（例如，电子邮件应用或社交共享应用）共享的数据，则应使用 Intent 将此操作与 startActivity() 结合使用。

  您可以使用 setAction() 或 Intent 构造函数为 Intent 指定操作。

  如果定义自己的操作，请确保将应用的软件包名称作为前缀。 例如：

        static final String ACTION_TIMETRAVEL = "com.example.action.TIMETRAVEL";
    
- **data**

  引用待操作数据和/或该数据MIME类型的URI（Uri对象）。提供的数据类型通常由Intent的操作决定。例如，如果操作是 ACTION_EDIT，则数据应包含待编辑文档的 URI。
  
  创建 Intent 时，除了指定URI以外，指定数据类型（其MIME类型）往往也很重要。例如，能够显示图像的 Activity 可能无法播放音频文件，即便URI格式十分类似时也是如此。因此，指定数据的MIME类型有助于 Android 系统找到接收Intent的最佳组件。但有时，MIME类型可以从URI中推断得出，特别当数据是 content: URI 时尤其如此。这表明数据位于设备中，且由ContentProvider控制，这使得数据MIME类型对系统可见。

  要仅设置数据 URI，请调用 setData()。 要仅设置MIME类型，请调用setType()。如有必要，您可以使用 setDataAndType() 同时显式设置二者。

  **注意**：若要同时设置URI和MIME类型，请勿调用setData()和setType()，因为它们会互相抵消彼此的值。请始终使用 setDataAndType() 同时设置 URI 和 MIME 类型。
  
- **type**

  一个包含应处理 Intent 组件类型的附加信息的字符串。 您可以将任意数量的类别描述放入一个 Intent 中，但大多数 Intent 均不需要类别。 以下是一些常见类别：
  
  - CATEGORY_BROWSABLE
  
    目标 Activity 允许本身通过网络浏览器启动，以显示链接引用的数据，如图像或电子邮件。

  - CATEGORY_LAUNCHER
  
    该 Activity 是任务的初始 Activity，在系统的应用启动器中列出。

  您可以使用 addCategory() 指定类别。
  
- **Extra**

  携带完成请求操作所需的附加信息的键值对。正如某些操作使用特定类型的数据 URI 一样，有些操作也使用特定的 extra。
  
  您可以使用各种 putExtra() 方法添加 extra 数据，每种方法均接受两个参数：键名和值。您还可以创建一个包含所有 extra 数据的 Bundle 对象，然后使用 putExtras() 将Bundle 插入 Intent 中。

  例如，使用 ACTION_SEND 创建用于发送电子邮件的 Intent 时，可以使用 EXTRA_EMAIL 键指定“目标”收件人，并使用 EXTRA_SUBJECT 键指定“主题”。

  Intent 类将为标准化的数据类型指定多个 EXTRA_* 常量。如需声明自己的 extra 键（对于应用接收的 Intent），请确保将应用的软件包名称作为前缀。 例如：

        static final String EXTRA_GIGAWATTS = "com.example.EXTRA_GIGAWATTS";
 
- **flag** 

  在 Intent 类中定义的、充当 Intent 元数据的标志。 标志可以指示 Android 系统如何启动 Activity（例如，Activity 应属于哪个任务），以及启动之后如何处理（例如，它是否属于最近的 Activity 列表）。

  
### 构建显式Intent

### 构建隐式Intent

### 应用选择器

## 应用组件声明接收的Intent

## Intent匹配应用组件




