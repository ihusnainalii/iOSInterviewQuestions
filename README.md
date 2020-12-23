# iOS Interview Questions and Answers for Senior Developers
I recently got into the position of leading the technical interview when my client was searching for a new senior iOS developer.
It's a challenging task to evaluate the skills and knowledge of another developer. So in this series, I want to share my results of the most useful iOS and Swift questions and answers with you. I categorised the questions into different topics:

1- Swift Programming Language
2- Networking
3- Persistence & Databases 
4- Concurrency
5- Architecture & Design Patterns


-------------------------------------- 
## 1. What do you like about Swift?
Examples of positive aspects the interviewed person could name are type safety, type inference, short and adept syntax, functional programming patterns, generics, protocol oriented programming, extensions, built-in error handling etc.

## 2. What is the difference between value and reference types?
Value types are usually defined as struct or enum and reference types as a class. When copying a value type, the variable will create a copy of its data. The reference type variable will just point to the original data in the memory.

## 3. What are optionals? How can you unwrap an optional in a safe way? What is forced unwrapping?
Optional types are used to handle the abscence of a value. You can use if let or guard statements to unwrap the optional and get the value safely.
Forced unwrapping provides direct access to the value by using the exclamation mark. When the optional has no value (is nil), forced anwrapping leads to a runtime error.

## 4. What are higher order functions? Give some examples of higher order functions in Swift. Explain one of them.
A higher order function is a function that takes one or more functions as arguments or returns a function as its result. Examples are map, filter and reduce. Higher order functions improve readability and decrease the length of code. E.g. map iterates through the array and changes each element of that array based on the closure passed to the method.

## 5. What benefits do generics have? How can you define a generic function in Swift?
Generic code enables you to avoid code duplication by writing flexible and reusable components. You can define a generic function by putting the placeholder type name inside angle brackets right after the function name.

func methodName<T>(parameterName: T)
You can then use this generic type for parameters or as return type. An indepth explanation with examples is available in the official Swift documentation.

## 6. What is a protocol associated type in Swift?
Associated types are a powerful way of making protocols generic. You can declare one or more associated types as part of the protocol’s definition. An associated type gives a placeholder name to a type that is used as part of the protocol. The actual type to use for that associated type is specified when the protocol is adopted.

## 7. What is protocol composition?
Protocol compositions can be created by combining two or more protocols with a & sign. This is really useful when you want a type to conform to multiple protocols at the same time.
func methodName(paramterName: ProtocolOne & ProtocolTwo)

## 8. What are opaque types in Swift? Where can opaque types be useful?
To define a type as opaque, you can use the some keyword.
func methodName() -> some ProtocolName
Opaque types give developers the capability to return a concrete type without having to expose it. Returning an opaque type is almost like returning a protocol. In both cases, the caller cannot see the concrete type. The difference is that unlike protocols, an opaque type still refers to a specific type.
An indepth explanation with examples is available in the official Swift documentation.

## 9. What is Automatic Reference Counting in Swift? Explain how it works.
Swift uses Automatic Reference Counting (ARC) to manage the memory usage of the app. Every time a new class instance is created, ARC allocates memory to store it. To free up the allocated memory, it needs to determine when an instance is no longer needed. To do that, ARC counts the strong references of each instance. It will deallocate the instance when it's count of strong references drops to zero.

## 10. Explain the difference between a weak, unowned and a strong reference?
Creating a strong reference to an instance will increase it's reference count.
A weak or an unowned reference only points to an object and does not increase it's reference count.
The difference between a weak and an unowned reference is that a weak reference allows the object to become nil. An unowned reference presumes that it will never become nil during it's lifetime.

## 11. What is a memory leak? How can you avoid them? Explain your process for tracing and fixing a memory leak.
A memory leak is created when memory cannot be deallocated due to retain cycles. A retain cycle is created when two instances keep a strong reference to each other so ARC cannot release any of them. Memory leaks can be avoided by using a weak or unowned reference when needed.
For detecting memory leaks, Xcode offers a built-in debug memory graph. An alternative way is the Instruments tool. Xcode's debug memory graph is easier to use, Instruments offers a more in-depth look into memory leaks.

-------------------------------------- 
## 1. What are the main components of a HTTP URL? What purpose do they have?
Every HTTP URL consists of the following components:
scheme://host:port/path?query

    Scheme - The scheme identifies the protocol used to access a resource, e.g. http or https.
    Host - The host name identifies the host that holds the resource.
    Port - Host names can optionally be followed by a port number to specify what service is being requested.
    Path - The path identifies a specific resource the client wants to access.
    Query - The path can optionally by followed by a query to specify more details about the resource the client wants to access.

## 2. Give some examples of HTTP request methods you know. What purpose do they have?
HTTP defines a set of request methods to indicate the desired action to be performed for a given resource. Examples are:

    GET - retrieve a resource
    POST - create a resource
    PUT - update a resource
    DELETE - delete a resource

Other HTTP request methods are HEAD, OPTIONS, PATCH etc.

## 3. How would you create a HTTP request in iOS?
One way of creating a HTTP request in iOS is by using URLRequest with URLComponents.
var components = URLComponents()
components.scheme = "https"
components.host = "api.github.com"
components.path = "/search/repositories"
components.queryItems = [
    URLQueryItem(name: "q", value: "swift"),
    URLQueryItem(name: "sort", value: "stars")
]

let url = components.url
With URLComponents you can easily define the URL components and then create a URLRequest out of it.

## 4. How would you send and receive HTTP requests in iOS?
One possibility for sending and receiving HTTP requests is by using URLSession. With URLSessionConfiguration, you can configure caching behaviour, timeout values and HTTP headers.
A session works with tasks. After creating a session you can use URLSessionDataTask to send a request and to get the response:
let urlSession = URLSession(configuration: .default)
let task = urlSession.dataTask(with: url) { data, response, error in
    // Handle response.
}
task.resume()
You can also use URLSessionDownloadTask to download and URLSessionUploadTask to upload files. You can suspend, resume and cancel tasks.

## 5. The server response contains a HTTP status code. Give some examples and explain their meaning.
The HTTP response status codes are separated into five categories:

    1xx Informational: Request was received, process is continuing.
    2xx Successful: Request was successfully received, understood and accepted.
    3xx Redirection: Further action needs to be taken to complete the request.
    4xx Client Error: Request contains bad syntax or cannot be fulfilled.
    5xx Server Error: Server failed to fulfill an apparently valid request.

Examples:

    200 OK: Request was successful.
    201 Created: Request was successful, a new resource was created.
    400 Bad Request: Server did not understand the request.
    401 Unauthorized: Authentication has failed.

## 6. In iOS, a networking feature called App Transport Security (ATS) requires that all HTTP connections use HTTPS. Why?
HTTPS (Hypertext Transfer Protocol Secure) is an extension of HTTP. It is used for secure computer network communication. In HTTPS, the communication protocol is encrypted using TLS (Transport Layer Security).
The goal of HTTPS is to protect the privacy and integrity of the exchanged data against eaves-droppers and man-in-the-middle attacks. It is achieved through bidirectional encryption of communications between a client and a server.
So through ATS, Apple is trying to ensure privacy and data integrity for all apps. There is a way to circumvent these protections by setting NSAllowsArbitraryLoads to true in the app’s Information Property List file. But Apple will reject apps who use this flag without a specific reason.


## 7. RESTful APIs typically return the response data in JSON format. What value types are supported by a JSON schema?
JSON supports four basic value types and two complex data types.
The basic types are string, number, boolean and null.
The complex data types are object and array. An object is an unordered set of name/value pairs. An array is an ordered collection of values.


## 8. How would you convert a JSON response into native Swift types?
Swift’s Codable protocol makes it easy to convert JSON to native Swift structs and classes. The first step is to define a Swift type that has the same keys and value types as the JSON and conforms to Codable. For example:
struct Doggy: Codable {
	let name: String 
	let age: Int 
}

Then we can use JSONDecoder to convert:
let decoder = JSONDecoder()
do {
    let doggies = try decoder.decode([Doggy].self, from: jsonData)
    print(people)
} catch {
    print(error.localizedDescription)
}

## 9. What is the basic idea behind the OAuth protocol?
OAuth lets users grant third-party services access to their web resources, without sharing their passwords. This is possible though a security object known as access token.
If a third-party app wants to connect to the main service, it must get its own access token. The users password is kept safe inside the main service and cannot be obtained from the access token. Access tokens can then be revoked if the user does not want to use the third-party app anymore.


## 10. Do you have experience with third-party iOS networking libraries? If so, which ones?
The interviewed person could name following libraries:
Alamofire - an abstraction layer over URLSession to make networking more simple and elegant
Moya - an abstraction layer over Alamofire that creates a type-safe structure for network services and requests
Of course, there are a lot more iOS networking libraries. 

-------------------------------------- 
## 1. What options to persist data are available on iOS?
Depending on the case and the amount of data, there are different ways to store data. Examples are User Defaults, saving Files, Keychain, Core Data. There are also third party libraries like Realm.

## 2. In what cases would you use UserDefaults?
UserDefaults is a great way to save a small amount of data. You can store basic types such as Bool, Int, String etc., and also more complex types like arrays and dictionaries.

## 3. When would you use the Keychain to store data?
Keychain is a good choice, if you want to save highly sensitive and secure data like passwords, keys etc. When using Keychain, it automatically takes care of data encryption before it is stored in the file system.

## 4. How would you persist large amounts of data objects with relationships in iOS?
Core Data or Realm Database are often used on iOS to deal with a large amount of data.
Core Data is Apple’s native solution for persistence. Through Core Data’s model editor, you define your data’s types and relationships, and generate corresponding class definitions.
Realm Database is a highly supported open source library that is also available for Android. It is popular because it is easy to use and really fast.
These are not the only options of course. For example, you could also use a SQL database, there are some SQL libraries around, like SQLite.swift.

## 5. Explain what data migration means and when it needs to be done?
Data migration is often necessary when you need to make changes to the data model. So every time when the data model needs a change, e.g. for a new feature request, you’ll need to create a new version of the data model and provide a migration path.
An alternative to migration is simply deleting and rebuilding the data store. But this is only an option, if the stored user data can fully be restored, e.g. from a backend api.

-------------------------------------- 

## 1. What possibilities do you have to run code on a background thread in iOS?
The first possibility is to use the old plain threads, but by using them directly, you would have to deal with a low-level API.
A higher level of abstraction is provided by Grand Central Dispatch (GCD). When using GCD, you can execute blocks of code on a DispatchQueue - either one you create by yourself or one provided by the iOS system.
Another possiblity is to use Operations which are built on top of GCD.

## 2. What is the difference between a serial and a concurrent queue?
Serial queues execute one task at a time in the order in which they are added to the queue. So a serial queue guarantees that a task will finish first before another task will start. Concurrent queues allow multiple tasks to run at the same time. The code below shows how these two types of queues are created.
let serialQueue = DispatchQueue(label: "serialQueue")
let concurrentQueue = DispatchQueue(label: "concurrentQueue", attributes: .concurrent)

## 3. When you review the code below, would you suggest an improvement?
DispatchQueue.global(qos: .background).async {
    let text = loadDescription()
    label.text = text
}
Yes. UIKit can only be used from our app’s main queue, so the label's text shouldn't be set on a background queue. One possible way to solve this is to call the UI update code on the main queue.
DispatchQueue.global(qos: .background).async {
    let text = loadDescription()
    DispatchQueue.main.async {
        label.text = text
    }
}

## 4. What is the difference between asynchronous and synchronous tasks?
Synchronously starting a task blocks the calling thread until the task is finished. Asynchronously starting a task returns directly on the calling thread without blocking.

## 5. How can you cancel a running asynchronous task?
In GCD, you can achieve that with DispatchWorkItem. By setting it up with the task that needs to be done asynchroniously and calling the cancel method when needed.
let workItem = DispatchWorkItem { [weak self] in
    // Execute time consuming code.
}
// Start the task.
DispatchQueue.main.async(execute: workItem)
// Cancel the task.
workItem.cancel()

By using Operations, it's almost the same as with GCD. You can use the cancel method on the operation.
let queue = OperationQueue()
let blockOperation = BlockOperation {
    // Execute time consuming code.
}
// Start the operation.
queue.addOperation(blockOperation)
// Cancel the operation.
blockOperation.cancel()

## 6. How can you group asynchronous tasks?

By using GCD, you can use DispatchGroup to achieve this.

let group = DispatchGroup()

// The 'enter' method increments the group's task count.
group.enter()
dataSource1.load { result in
    // Save the result.
    group.leave()
}

group.enter()
dataSource2.load { result in
    // Save the result.
    group.leave()
}

// This closure is called when the group's task count reaches 0.
group.notify(queue: .main) { [weak self] in
    // Show the loaded results.
}

When working with operations, you can add dependencies between them.

let operation1 = BlockOperation {
    // Execute time consuming code.
}
let operation2 = BlockOperation {
    // Execute time consuming code.
}
let operation3 = BlockOperation {
    // operation1 and operation2 are finished.
}
operation3.addDependency(operation1)
operation3.addDependency(operation2)

## 7. What is a race condition?
A race condition can occur when multiple threads access the same data without synchronization and at least one access is a write. For example if you read values from an array from the main thread while a background thread is adding new values to that same array.
Data races can be the root cause behind unpredicatable program behaviours and weird crashes that are hard to find.


## 8. How can you avoid race conditions?
To avoid race conditions you can access the data on the same serial queue or you can use a concurrent queue with a barrier flag. A barrier flag makes access to the data thread-safe.

var queue = DispatchQueue(label: "messages.queue", attributes: .concurrent)

queue.sync {
    // Code for accessing data
}

queue.sync(flags: .barrier) {
    // Code for writing data
}

The barrier flag approach has the benefit of synchronizing write access while reading can still be done concurrently. By using a barrier, a concurrent queue becomes a serial queue for a moment. A task with a barrier is delayed until all running tasks are finished. When the last task is finished, the queue executes the barrier block and continues it's concurrent behavior after that.


## 9. How can you find and debug race conditions?
You can use Apples Thread Sanitizer to debug race conditions, it detects them at runtime. The Thread Sanitizer can be enabled from the scheme configuration.


-------------------------------------- 

## 1. What is the idea behind the MVC architecture?
MVC stands for Model View Controller.

    the View represents the User Interface layer
    the Model represents the Data Access layer
    the Controller represents the Business Logic layer

The controller is the mediator between the view and the model. The model and the view know nothing about the controller or each other. They communicate with the outside world by sending a notification about some event, for example by calling a callback method or using the observer pattern.


## 2. Which alternative architectures do you know? Explain one of them.

Examples of alternative architectures are MVVM (Model-View-ViewModel), MVP (Model-View-Presenter) or VIPER (View-Interactor-Presenter-Entity-Routing).
The MVVM architecture consists of a Model, a View and a ViewModel. The View and the Model are already familiar from MVC and have the same responsibilities. The mediator between them is represented by the View Model.
In theory, you could say that MVC and MVVM are the same architectures with different namings for the mediator. In practice however, many developers ran into the Massive View Controller problem with Apple's MVC by using a UIViewController as the Controller. But UIViewControllers are too involved in the View’s life cycle so it’s hard to separate them.
The main difference between the ViewModel from MVVM and the Controller from MVC is that the ViewModel is defined to be UIKit independent. With MVVM, the UIViewController becomes part of the View.


## 3. Explain the observer design pattern. What options do you have to implement the observer design pattern in Swift?
The observer design pattern is characterized by two elements:
    A value being observed (an observable), which notifies all observers if a change happens.
    An observer, who subscribes to changes of the observable.
Existing solutions of the observer pattern in iOS are:
    Notifications
    Key-Value Observing
    Publishers (observables) and Subscribers (observers) of the Combine Framework

## 4. What is a singleton design pattern? When would you use and when would you avoid singletons?
The singleton design pattern ensures that only one instance exists for a given class.
In Swift, this pattern is easy to implement:
class ChocolateFactory {
    static let shared: ChocolateFactory = {
        let instance = ChocolateFactory()
        return instance
    }()
}

A singleton is useful when exactly one object is needed to coordinate actions across the app. A good example is Apple's UIApplication.shared instance, because within the app there should be only one instance of the UIApplication class.

The singleton pattern is easy to overuse though. In most cases, it can be replaced with dependecy injection, for example by passing the dependencies into the object's initializer. With the dependecy injection approach, modularity and testability of the code improves. It becomes a lot easier to mock and fake passed in dependencies for unit tests.


## 5. Name some other design patterns you know and give a short explanation.

Examples of other design patterns are the delegate design pattern, the factory design pattern and the facade design pattern.
The delegate design pattern allows an object to communicate back to its owner in a decoupled way. This way, an object can hand off (delegate) some of its responsibilities to its owner. A common way to define a delegate is by using protocols. The delegate design pattern is an old friend on Apple's platforms. Examples are UITableViewDelegate, UICollectionViewDelegate, UITextViewDelegate etc.

The factory design pattern is a way to encapsulate the implementation details of creating objects. It separates the creation from the usage of the objects. The initialization is done in a central place. So when the initialization logic of certain objects changes, you don't need to change every creation in the whole project.

Example of a simple factory:
class ChocolateFactory {
    static func produceChocolate(of type: ChocolateType) -> Chocolate {
        switch type {
        case .dark:
            return DarkChocolate()
        case .raw:
            return RawChocolate()
        }
    }
}

The facade design pattern provides a simpler interface for a complex subsystem. Instead of exposing a lot of classes and their APIs, you only expose one unified API. The facade pattern improves the readability and usability. It also decouples and reduces dependencies. If implementation details under the facade change, the facade can retain the same API while things change behind the scenes.

For example, you could create a ChocolateShopService and hide all the http requests and persistence details behind it.

class ChocolateShopService {

    func getChocolates() -> [Chocolate] {
        if isOnline {
            return httpClient.get("/api/chocolates")
        } else {
            return localStore.getChocolates()
        }
    }
}

## 6. How can you avoid the problem of so called spaghetti code?

The term spaghetti code is used for unstructured and difficult-to-maintain code. To avoid spaghetti code it is important to constantly think about building clean, reusable and testable components.
Those qualities can be achieved by focusing on decoupling and separation of concerns, for example through dependency injection, generic solutions, protocol oriented programming and by using appropriate design patterns.


## 7. What is your undestanding of reactive programming? What possibilities do you have on iOS?

Reactive programming is programming with asynchronous observable streams.
A stream (also called an observable) can emit either a value of some type, an error or a completed event. You can listen to a stream by subscribing to it (observing it).
You can observe those async streams and react with various functional methods when a value is emitted. You can merge two streams, filter a stream to get another one or map data values from one stream to another new one.
To use reactive programming in iOS you can use the Combine framework that was introduced at WWDC 2019. Or you can use a third party library like RxSwift.

## 8. What is TDD? What benefits and limitations does it have?
TDD stands for test driven development. The idea behind it is to write tests before writing code.
Benefits Test driven development leads to higher code test coverage. Also, the tests provide documentation about how the app is expected to behave. TDD helps to build modularized code, because it requires the developer to think in small units that can be written and tested independently. This leads to decoupled focused components and clean interfaces.

Limitations TDD only focuses on unit tests, it does not cover UI or integration tests. So additional tests are needed to make sure that the app is working properly. When using TDD, code design decisions become even more important than they already are, because tests become part of the maintenance overhead. Badly written tests are prone to failure and expensive to maintain.

## 9. What are good use cases to use extensions in Swift?
An extension in Swift is a powerful way to add new functionality to existing classes, structures or enums without modifying its code.
For example, accessing an array element leads to a crash if the specified index doesn't exist. To avoid a crash, you could wrap every access in an if-block. A more elegant solution with an extension looks like this:
extension Array {
    /// Returns the element at the specified index if it is within bounds, otherwise nil.
    subscript(safe index: Index) -> Iterator.Element? {
        return indices.contains(index) ? self[index] : nil
    }
}

// Usage:
let value = someArray[safe: 6]

## 10. What is your understanding of protocol oriented programming?
Protocol oriented programming is a concept of designing your code base by using protocols. In Swift, protocols provide an extensive set of features, so they have become a powerful way of managing code complexity and creating modular testable components.
A protocol allows you to group similar methods and properties for classes, structs and enums. In contrast to class inheritence, objects can conform to multiple protocols.
Like classes, protocols also support inheritence. For example, the Comparable protocol inherits from Equatable in Swift. You can also combine two protocols together to build new protocols, for example:
typealias Codable = Decodable & Encodable
To implement default behaviour with protocols, you can use protocol extensions.

protocol Chocolate {
    var hasSugar: Bool { get }
}

extension Chocolate {
    var hasSugar: Bool {
        return true
    }
}

struct DarkChocolate: Chocolate {
    // hasSugar is true by default
}

This way, all types that conform to the same protocol will get the default behaviour.


## 11. Explain dependency injection. What types of dependency injection do you know?
Dependency injection is a technique where an object gets its dependencies from the outside instead of creating the dependency internally. With dependency injection, the objects get less coupled, more reusable and more testable, it gets a lot easier to mock objects for unit tests.
Dependency injection can be implemented in different ways, for example an initializer-based or a property-based dependency injection.
There are also some external libraries like Swinject or Dip that centralize the managing of dependencies.
