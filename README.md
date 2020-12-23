# iOS Interview Questions and Answers for Senior Developers
I recently got into the position of leading the technical interview when my client was searching for a new senior iOS developer.
It's a challenging task to evaluate the skills and knowledge of another developer. So in this series, I want to share my results of the most useful iOS and Swift questions and answers with you. I categorised the questions into different topics:

1- Swift Programming Language
2- Networking
3- Persistence & Databases 
4- Concurrency
5- Architecture & Design Patterns


# -------------------------------------- Swift Programming Language --------------------------------------------
1. What do you like about Swift?
Examples of positive aspects the interviewed person could name are type safety, type inference, short and adept syntax, functional programming patterns, generics, protocol oriented programming, extensions, built-in error handling etc.

2. What is the difference between value and reference types?
Value types are usually defined as struct or enum and reference types as a class. When copying a value type, the variable will create a copy of its data. The reference type variable will just point to the original data in the memory.

3. What are optionals? How can you unwrap an optional in a safe way? What is forced unwrapping?
Optional types are used to handle the abscence of a value. You can use if let or guard statements to unwrap the optional and get the value safely.
Forced unwrapping provides direct access to the value by using the exclamation mark. When the optional has no value (is nil), forced anwrapping leads to a runtime error.

4. What are higher order functions? Give some examples of higher order functions in Swift. Explain one of them.
A higher order function is a function that takes one or more functions as arguments or returns a function as its result. Examples are map, filter and reduce. Higher order functions improve readability and decrease the length of code. E.g. map iterates through the array and changes each element of that array based on the closure passed to the method.

5. What benefits do generics have? How can you define a generic function in Swift?
Generic code enables you to avoid code duplication by writing flexible and reusable components. You can define a generic function by putting the placeholder type name inside angle brackets right after the function name.

func methodName<T>(parameterName: T)
You can then use this generic type for parameters or as return type. An indepth explanation with examples is available in the official Swift documentation.

6. What is a protocol associated type in Swift?
Associated types are a powerful way of making protocols generic. You can declare one or more associated types as part of the protocol’s definition. An associated type gives a placeholder name to a type that is used as part of the protocol. The actual type to use for that associated type is specified when the protocol is adopted.

7. What is protocol composition?
Protocol compositions can be created by combining two or more protocols with a & sign. This is really useful when you want a type to conform to multiple protocols at the same time.
func methodName(paramterName: ProtocolOne & ProtocolTwo)

8. What are opaque types in Swift? Where can opaque types be useful?
To define a type as opaque, you can use the some keyword.
func methodName() -> some ProtocolName
Opaque types give developers the capability to return a concrete type without having to expose it. Returning an opaque type is almost like returning a protocol. In both cases, the caller cannot see the concrete type. The difference is that unlike protocols, an opaque type still refers to a specific type.
An indepth explanation with examples is available in the official Swift documentation.

9. What is Automatic Reference Counting in Swift? Explain how it works.
Swift uses Automatic Reference Counting (ARC) to manage the memory usage of the app. Every time a new class instance is created, ARC allocates memory to store it. To free up the allocated memory, it needs to determine when an instance is no longer needed. To do that, ARC counts the strong references of each instance. It will deallocate the instance when it's count of strong references drops to zero.

10. Explain the difference between a weak, unowned and a strong reference?
Creating a strong reference to an instance will increase it's reference count.
A weak or an unowned reference only points to an object and does not increase it's reference count.
The difference between a weak and an unowned reference is that a weak reference allows the object to become nil. An unowned reference presumes that it will never become nil during it's lifetime.

11. What is a memory leak? How can you avoid them? Explain your process for tracing and fixing a memory leak.
A memory leak is created when memory cannot be deallocated due to retain cycles. A retain cycle is created when two instances keep a strong reference to each other so ARC cannot release any of them. Memory leaks can be avoided by using a weak or unowned reference when needed.
For detecting memory leaks, Xcode offers a built-in debug memory graph. An alternative way is the Instruments tool. Xcode's debug memory graph is easier to use, Instruments offers a more in-depth look into memory leaks.

# ------------------------------------- Networking ---------------------------------------------
1. What are the main components of a HTTP URL? What purpose do they have?
Every HTTP URL consists of the following components:
scheme://host:port/path?query

    Scheme - The scheme identifies the protocol used to access a resource, e.g. http or https.
    Host - The host name identifies the host that holds the resource.
    Port - Host names can optionally be followed by a port number to specify what service is being requested.
    Path - The path identifies a specific resource the client wants to access.
    Query - The path can optionally by followed by a query to specify more details about the resource the client wants to access.

2. Give some examples of HTTP request methods you know. What purpose do they have?

HTTP defines a set of request methods to indicate the desired action to be performed for a given resource. Examples are:

    GET - retrieve a resource
    POST - create a resource
    PUT - update a resource
    DELETE - delete a resource

Other HTTP request methods are HEAD, OPTIONS, PATCH etc.
3. How would you create a HTTP request in iOS?

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
4. How would you send and receive HTTP requests in iOS?

One possibility for sending and receiving HTTP requests is by using URLSession. With URLSessionConfiguration, you can configure caching behaviour, timeout values and HTTP headers.

A session works with tasks. After creating a session you can use URLSessionDataTask to send a request and to get the response:

let urlSession = URLSession(configuration: .default)
let task = urlSession.dataTask(with: url) { data, response, error in
    // Handle response.
}
task.resume()

You can also use URLSessionDownloadTask to download and URLSessionUploadTask to upload files. You can suspend, resume and cancel tasks.
5. The server response contains a HTTP status code. Give some examples and explain their meaning.

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

6. In iOS, a networking feature called App Transport Security (ATS) requires that all HTTP connections use HTTPS. Why?

HTTPS (Hypertext Transfer Protocol Secure) is an extension of HTTP. It is used for secure computer network communication. In HTTPS, the communication protocol is encrypted using TLS (Transport Layer Security).

The goal of HTTPS is to protect the privacy and integrity of the exchanged data against eaves-droppers and man-in-the-middle attacks. It is achieved through bidirectional encryption of communications between a client and a server.

So through ATS, Apple is trying to ensure privacy and data integrity for all apps. There is a way to circumvent these protections by setting NSAllowsArbitraryLoads to true in the app’s Information Property List file. But Apple will reject apps who use this flag without a specific reason.
7. RESTful APIs typically return the response data in JSON format. What value types are supported by a JSON schema?

JSON supports four basic value types and two complex data types.

The basic types are string, number, boolean and null.

The complex data types are object and array. An object is an unordered set of name/value pairs. An array is an ordered collection of values.
8. How would you convert a JSON response into native Swift types?

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

9. What is the basic idea behind the OAuth protocol?

OAuth lets users grant third-party services access to their web resources, without sharing their passwords. This is possible though a security object known as access token.

If a third-party app wants to connect to the main service, it must get its own access token. The users password is kept safe inside the main service and cannot be obtained from the access token. Access tokens can then be revoked if the user does not want to use the third-party app anymore.
10. Do you have experience with third-party iOS networking libraries? If so, which ones?

The interviewed person could name following libraries:
Alamofire - an abstraction layer over URLSession to make networking more simple and elegant
Moya - an abstraction layer over Alamofire that creates a type-safe structure for network services and requests
Of course, there are a lot more iOS networking libraries. 

