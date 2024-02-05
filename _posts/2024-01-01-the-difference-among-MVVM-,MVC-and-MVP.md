---
layout: post
title: the difference among MVVM ,MVC and MVP
image: 
  path: /assets/img/blog/MVVM.png
description: >
  There are two main ways in which the view of a website manipulates the model of a website. that’s either with a **controller** or with a **View Model**.
sitemap: false
---


# the difference among MVVM ,MVC and MVP


Although the logic that’s necessary to run an application like Facebook is different  from the logic that’s necessary to run an application like google, at its core, any website’s functionality is simply a way in which the front end or the view can reach the appropriate model to retrieve data. In any case, there will always be a model and there will always be a view. **What really changes is the way in which the models and views are connected.** 

There are two main ways in which the view of a website manipulates the model of a website. that’s either with a **controller** or with a **View Model**.

The common point is that these architectures are designed to **separate the view from the model.**

尽管运行类似Facebook的应用程序所需的逻辑与运行类似Google的应用程序所需的逻辑不同，但在其核心，任何网站的功能都只是前端或视图以某种方式访问适当的模型以检索数据的方式。无论如何，始终会有一个模型，始终会有一个视图。**真正改变的是模型和视图连接的方式。**

网站的视图操作网站的模型有两种主要方式，即使用**控制器**或**视图模型**。

这些体系结构的共同点在于它们旨在**将视图与模型分离**。

## MVC: Model-View-Controller

the most standard way in which the data model is connected to the view of an application is through an interface called a controller. In the MVC pattern the controller **acts as a tool** **that directly manipulates the data in its given model.**

Nowadays, the frontend and backend of websites are set up to be completely decoupled from one and another. the view component of the MVC pattern to be stored within the same file as its data models has become hard because of more and more complex user interface of websites. t**he frontend and backend is connected only through get/post requests and JSON strings that are organized through controllers and a router.**

将数据模型与应用程序的视图连接的最标准方式是通过一个称为控制器的接口。在MVC模式中，控制器**充当一个工具**，**直接操纵其给定模型中的数据。**

如今，网站的前端和后端被设置为完全解耦。由于网站用户界面越来越复杂，将MVC模式的视图组件存储在与其数据模型相同的文件中变得困难。**前端和后端仅通过get/post请求和通过控制器和路由器组织的JSON字符串相连接**

![MVC](/assets/img/blog/MVC.png)  

```jsx
onLogin = () => {
  const loginParams = Object.assign({},
        {user_name:this.state.user_name},
        {password: this.state.password});
 fetch('',{
		method: 'POST',
		body: JSON.stringify(loginParams)
		}).then(res => res.json())
      .then(current_user => this.setState({current_user}))
}
```

this is typical login fetch request looks like in React which is accomplished **within** **the view by fetch requests** that **hit specific routes on the API that are tied to controller actions.**

Each controller is designed to **both receive data** and **send back appropriate information** based on the data received.

In the MVC pattern, the view and the model don’t need to know anything about each other.

这是在React中实现的典型登录获取请求，它是在视图中通过fetch请求实现的，这些请求**命中与控制器动作相关联的API上的特定路由。**

每个控制器都被设计为**接收数据**并基于接收到的数据**发送适当的信息回去**。

在MVC模式中，视图和模型不需要彼此了解。

> 
> 
> - The model is responsible for managing the data of the application. It receives user input from the controller.
> - The view renders presentation of the model in a particular format.
> - The controller responds to the user input and performs interactions on the data model objects. The controller receives the input, optionally validates it and then passes the input to the model

## MVVM: Model-View-ViewModel

As in the MVC and MVP patterns, the views and models are similar. Model refers to the data, logic and rules of the application. the view is the structure, layout, and appearance of what a user sees on the screen. It displays a representation of the model and receives the user’s interaction with the view(mouse clicks, keyboard input etc), and **it forwards the handling of these to the view model via the data binding( properties, event callbacks, etc) that is defined to link the view and view model.**

Unlike the controller of the MVC pattern, or the presenter of the MVP pattern, MVVM has a binder, **which automates communication between the view and its bound properties in the view model.** the view model has been described as a state of the data in the model.

the main difference between the view model and the presenter in the MVP pattern is that the **presenter has a reference to a view,** whereas the view model does not. Instead , a view directly **binds to properties on the view model to send and receive updates.** This requires a binding technology or the generation of boilerplate code to efficiently enable the functionality.

Because of data binding, MVVM single page application can move quickly and fluidly and save information to the database continuously. However, because it relies on data binding, the View Model **consumes a considerable amount of memory** in comparison to it’s controlling counterparts.

the overhead for implementing MVVM is “overkill” for simple UI operations. Larger applications that use the ViewModel method regularly become incredibly difficult to run. For this reason, the MVVM design pattern is used mostly for single page/function applications on the web.

就像在MVC和MVP模式中一样，视图和模型是相似的。模型涵盖应用程序的数据、逻辑和规则，而视图是用户在屏幕上看到的结构、布局和外观。它显示了模型的表示，并接收用户与视图的交互（鼠标点击、键盘输入等），**通过数据绑定（属性、事件回调等）将这些交互传递给视图模型。**

与MVC模式的控制器或MVP模式的Presenter不同，MVVM具有一个绑定器，自动化了视图和其在视图模型中绑定属性之间的通信。视图模型被描述为模型中数据的状态。

视图模型和MVP模式中的Presenter之间的主要区别在于**Presenter有一个对视图的引用，**而视图模型没有。相反，视图**直接绑定到视图模型的属性以发送和接收更新。**这需要绑定技术或生成样板代码来有效地启用此功能。

由于数据绑定，MVVM单页应用程序可以快速、流畅地移动，并不断保存信息到数据库。然而，由于依赖于数据绑定，与其控制对应物相比，视图模型**消耗大量内存。**

对于简单的UI操作来说，实施MVVM的开销有些“过度”。经常使用ViewModel方法的较大应用程序变得难以运行。因此，MVVM设计模式主要用于Web上的单页/功能应用程序。

![MVVM](/assets/img/blog/MVVM.png)  

---

- **Model**: This represents the data model that your app consumes. For example, in a picture sharing app, this layer might represent the set of pictures available on a device and the API used to read and write to the picture library.
- **View**: An app typically is composed of multiple pages of UI. Each page shown to the user is a view in MVVM terminology. The view is the XAML code used to define and style what the user sees. The data from the model is displayed to the user, and it’s the job of the ViewModel to feed the UI this data based on the current state of the app. For example, in a picture sharing app, the views would be the UI that show the user the list of albums on the device, the pictures in an album, and perhaps another that shows the user a particular picture.
- **ViewModel**: The ViewModel ties the data model, or simply the model, to the UI, or views, of the app. It contains the logic with which to manage the data from the model and exposes the data as a set of properties to which the XAML UI, or views, can bind. For example, in a picture sharing app, the ViewModel would expose a list of albums, and for each album expose a list of pictures. The UI is agnostic of where the pictures come from and how they are retrieved. It simply knows of a set of pictures as exposed by the ViewModel and shows them to the user.

## MVP: Model -View -Presenter

Both MVP (Model-View-Presenter) and MVVM (Model-View-ViewModel) are derived from the MVC (Model-View-Controller) architecture. While they share the goal of separating the model and view to minimize coupling and enhance project scalability, the primary distinction lies in the way the view and model interact.

In the MVP pattern, the presenter acts as an intermediary between the view and model. It receives user input from the view, performs necessary operations on the model, and updates the view accordingly. The presenter maintains a reference to the view and controls its behavior. This approach enables the view to remain passive and focus solely on rendering the UI, while the presenter handles the logic and data manipulation.

On the other hand, the MVVM pattern introduces the concept of a view model, which serves as an abstraction layer between the view and model. The view model encapsulates the state and behavior of the view, exposing properties and commands that the view can bind to. Through data binding, any changes in the view model are automatically reflected in the view, and vice versa. This two-way communication allows for seamless synchronization between the UI and underlying data.

Unlike MVP, the view model in MVVM does not have a reference to the view. Instead, the view directly binds to properties on the view model, eliminating the need for explicit communication between the two. This decoupling enhances testability and maintainability, as the view model can be easily unit tested without relying on a specific view implementation.

It is essential to consider the communication mechanism implemented in the middle layer of both patterns. The way the view and model interact can vary depending on the specific application requirements. Whether it is through direct method calls, events, or data binding, the communication approach should be chosen carefully to ensure efficient and effective coordination between the view and model.

In summary, both MVP and MVVM are architectural patterns that aim to separate the concerns of the model and view, promoting code modularity and flexibility. While MVP relies on a presenter to facilitate communication, MVVM introduces a view model that enables data binding for seamless synchronization. Understanding the nuances of each pattern and selecting the appropriate one for a given project is crucial for creating robust and maintainable applications.

在MVP模式中，Presenter充当视图和模型之间的中介。它从视图接收用户输入，在模型上执行必要的操作，然后相应地更新视图。Presenter维护对视图的引用并控制其行为。这种方法使视图保持被动，专注于渲染UI，而Presenter处理逻辑和数据操作。

另一方面，MVVM模式引入了视图模型的概念，它充当视图和模型之间的抽象层。视图模型封装了视图的状态和行为，暴露属性和命令供视图绑定。通过数据绑定，视图模型中的任何更改都会自动反映在视图中，反之亦然。这种双向通信实现了UI和底层数据之间的无缝同步。

与MVP不同，MVVM中的视图模型不引用视图。相反，视图直接绑定到视图模型的属性，消除了两者之间显式通信的需要。这种解耦增强了可测试性和可维护性，因为可以轻松地对视图模型进行单元测试，而无需依赖于特定的视图实现。

在两种模式的中间层中实现的通信机制至关重要。视图和模型之间的交互方式可能因特定应用要求而异。无论是通过直接方法调用、事件还是数据绑定，都应仔细选择通信方法，以确保视图和模型之间的协调高效而有效。

总之，MVP和MVVM都是旨在分离模型和视图关注点、促进代码模块化和灵活性的架构模式。虽然MVP依赖于Presenter来促进通信，MVVM引入了视图模型，实现了数据绑定以实现无缝同步。理解每种模式的细微差别并为给定项目选择合适的模式对于创建健壮和可维护的应用程序至关重要