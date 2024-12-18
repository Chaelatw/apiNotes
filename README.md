# apiNotes

# Rest - Ch. 1

What is REST?
-
REST stands for:
    **R**epresentational
    **S**tate 
    **T**ransfer
- an architectural style for designing distributed
  network applications

Fundamental to REST is the concept of resource

Resource:
anything that can be accessed or
manipulated.
ex: 
- images
- videos
- persons
- devices

The Web provides the Uniform Resource Identifier, or URI, for uniquely identifying resources.
The syntax of a URI is:
scheme:scheme-specific-part
The scheme and the scheme-specific-part are separated using a semicolon.
- examples:
  - http
  - mailto

  it is possible for a resource to have more than one URI.

URI aliases provide flexibility and added convenience such as having to type fewer characters to get to the resource.

REST components interact with a resource by transferring its representations back and forth. hey
never directly interact with the resource.

JSON has become the de facto standard for REST services. 

HTTP
-
HTTP standard provides eight HTTP methods that allow
clients to interact and manipulate resources. Some of the commonly used methods are GET, POST, PUT, and
DELETE.

Safe:

Safe methods are used to retrieve resources.
Methods such as GET or HEAD, which are used to retrieve information/resources from the server
are typically implemented as read-only operations without causing any changes to the server’s state.

Idempotency:

An operation is considered to be idempotent if it produces the same server state whether we apply it once
or any number of times.

- HTTP:
  - GET
  - HEAD
  - PUT
  - DELETE

Head:

The HEAD method allows a client to only retrieve the metadata associated with a resource. No resource
representation gets sent to the client. 

Delete:

The DELETE method requests a resource to be deleted.
Because DELETE method modifies the state of the system, it is not considered to be safe. However, the
DELETE method is considered idempotent; subsequent DELETE requests would still leave the resource and
the system in the same state

Put:

The PUT method allows a client to modify a resource state

POST:

A POST request doesn’t need to know the URI of the resource. The server is responsible
for assigning an ID to the resource and deciding the URI where the resource is going to reside.


Patch:

is used to perform partial resource updates. It is neither safe nor idempotent.


Crud
-


Data-driven applications typically use the term C to indicate four basic persistence functions:

- Create -> POST 
- Update -> PUT 
- Read -> GET 
- Delete -> DELETE

HTTP Status Codes
-
The HTTP Status codes allow a server to communicate the results of processing a client’s request. 

- Informational Codes—Status codes indicating that the server has received the
  request but hasn’t completed processing it. These intermediate response codes are
  in the 100 series. 
- Success Codes—Status codes indicating that the request has been successfully
  received and processed. These codes are in the 200 series. 
- Redirection Codes—Status codes indicating that the request has been processed, but
  the client must perform an additional action to complete the request. These actions
  typically involve redirecting to a different location to get the resource. These codes
  are in the 300 series. 
- Client Error Codes—Status codes indicating that there was an error or a problem
  with client’s request. These codes are in the 400 series. 
- Server Error Codes—Status codes indicating that there was an error on the server while
  processing the client's request. These codes are in the 500 series.



| Status Code            | Description                                                                                                                  |
|------------------------|------------------------------------------------------------------------------------------------------------------------------|
| 100(Continue)          | Indicates that the server has received the first part of the request and the rest of the request should be sent              |
| 200(OK)                | Indicates that all went well with the request.                                                                               |
| 201(Created)           | Indicates that request was completed and a new resource got created                                                          |
| 202(Accepted)          | Indicates that request has been accepted but is still being processed.                                                       |
| 204(No content)        | Indicates that the server has completed the request and has no entity body to send to the client                             |
| 301(Moved Permanently) | Indicates that the requested resource has been moved to a new location and a new URI needs to be used to access the resource |

|                               |                                                                                                                                                                                                                    |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 400(Bad Request)              | Indicates that the request is malformed and the server is not able to understand the request.                                                                                                                      |
| 401 (Unauthorized)            | Indicates that the client needs to authenticate before accessing the resource. <br/> If the request already contains client’s credentials, then a 401 indicates invalid credentials (e.g., bad password)           |
| 403 (Forbidden)               | Indicates that the server understood the request but is refusing to fulfill it. <br/> This could be because the resource is being accessed from a blacklisted IP address or outside the approved time window.      |
| 404 (Not Found)               | Indicates that the resource at the requested URI doesn’t exist.                                                                                                                                                    |
| 406 (Not Acceptable)          | Indicates that the server is capable of processing the request; however, the generated response may not be acceptable to the client. <br/> This happens when the client becomes too picky with its accept headers. |
| 500 (Internal Server Error) I | Indicates that there was an error on the server while processing the request and that the request can’t be completed.                                                                                              |
| 503 (Service Unavailable)     | Indicates that the request can’t be completed, as the server is overloaded or going through scheduled maintenance.                                                                                                 |


# Spring Web MVC Primer - Ch. 2

The Spring Framework is made up of different modules that offer services such as data access, instrumentation, messaging, testing, and Web integration

Dependency Injection
-
Dependency Injection allows dependencies to be injected into components that need them.

Aspect Oriented Programming
-
Aspect Oriented Programming (AOP) is a programming model that implements crosscutting logic or
concerns. Logging, transactions, metrics, and security are some examples of concerns that span (crosscut)
different parts of an application.

AOP provides a standardized mechanism called an **aspect** for encapsulating such
concerns in a single location. The aspects are then weaved into other objects so that the crosscutting logic is
automatically applied across the entire application.


Model View Controller Pattern
-
The Model View Controller, or MVC, is an architectural pattern for building decoupled Web applications.

  - Model—The model represents data or state. In a Web-based banking application,
    information representing accounts, transactions, and statements are examples of
    the model. 

            public interface Model {
                    Model addAttribute(String attributeName, Object attributeValue);
                    Model addAttribute(Object attributeValue);
                    Model addAllAttributes(Collection<?> attributeValues);
                    Model addAllAttributes(Map<String, ?> attributes);        
                     Model mergeAttributes(Map<String, ?> attributes);
                    boolean containsAttribute(String attributeName);
                    Map<String, Object> asMap();
                }

                    @RequestMapping("/home.html")
                public String showHomePage(Model model) {
                model.addAttribute("currentDate", new Date());      
                return "home";
                }



    
  - View—Provides a visual representation of the model. This is what the user
      interacts with by providing inputs and viewing the output. In our banking
      application, Web pages showing accounts and transactions are examples of
      views. 


                    public interface View
                    {
                        String getContentType();
                        void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse
                    response) throws Exception;
                    }
              

                        @Controller
                        public class HomeController {
                            @RequestMapping("/home.html")
                            public View showHomePage() {
                                    JstlView view = new JstlView();
                                    view.setUrl("/WEB-INF/pages/home.jsp");
                                    return view;
                                }
                            }


  - Controller—The controller is responsible for handling user actions such as
    button clicks. It then interacts with services or repositories to prepare the model
    and hands the prepared model over to an appropriate view

                @Controller
                public class HomeController {
                    @RequestMapping("/home.html")
                    public String showHomePage() {
                            return "home";
                    }
                }

@RequestParam
-
The @RequestParam annotation is used to bind Servlet request parameters to handler/controller method
parameters.
The request parameter values are automatically converted to the specified method parameter
type using type conversion. 


            @RequestMapping("/search.html")
            public String search(@RequestParam String query, @RequestParam("page") int pageNumber) {
                    model.put("currentDate", new Date());
                    return "home";
            }


@RequestMapping
-

The @RequestMapping annotation is used to map a Web request to
a handler class or handler method.


            @RequestMapping(value="/saveuser.html", method=RequestMethod.POST)
            public String saveUser(@RequestParam String username, @RequestParam String password) {
                    // Save User logic
                    return "success";
                }


Path Variables
-

The @RequestMapping annotation supports dynamic URIs via URI templates


            @RequestMapping("/users/{username}")
            public User getUser(@PathVariable("username") String username) {
                    User user = null;
                    // Code to construct user object using username
                    return user;
                    }

View Resolver
-

When a logical view name is returned, a ViewResolver is employed to resolve the view to a View implementation. If this
process fails for some reason, a javax.servlet.ServletException is thrown. 


        public interface ViewResolver
            {
                    View resolveViewName(String viewName, Locale locale) throws Exception;
            }

Exception Handler
-
Exceptions are part of any application and Spring provides the HandlerExceptionResolver
mechanism for handling those unexpected exceptions

            public interface HandlerExceptionResolver {
            ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response,
            Object handler, Exception ex);
            }


Interceptors
-
Spring Web MVC provides the notion of interceptors to implement concerns that crosscut across different
handlers


            public interface HandlerInterceptor{
                void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object
            handler, Exception ex);
                void postHandle(HttpServletRequest request, HttpServletResponse response, Object
            handler, ModelAndView modelAndView);
                boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object
            handler);
            }

# RESTful Spring - Ch. 3

Generating a Spring Boot Project
-

- Use Spring Boot’s starter website (http://start.spring.io)
- Use the Spring Tool Suite (STS) IDE
- Use the Boot command line interface (CLI)


Installing a Build Tool
-

1. Download the latest Maven binary from http://maven.apache.org/download.cgi.
   At the time of writing this book, the current version of Maven was 3.2.5. For
   Windows, download the apache-maven-3.2.5-bin.zip file.
2. Unzip the contents of the zip file under C:\tools\maven.
3. Add an Environment variable M2_HOME with value C:\tools\maven\apachemaven-3.2.5-bin\apache-maven-3.2.5. This tells Maven and other tools where
   Maven is installed. Also make sure that the JAVA_HOME variable is pointing to the
   installed JDK.
4. Append the value %M2_HOME%\bin to the Path environment variable. This allows
   you to run Maven commands from the command line.
5. Open a new command line and type the following:
   mvn - v


Generating a Project using start.spring.io
-

Spring Boot hosts an Initializr application at http://start.spring.io

1. Launch the http://start.spring.io website in your browser and enter the
   information
2. Under Project dependencies ➤ Web, select the option “Web” and indicate
   that you would like Spring Boot to include Web project infrastructure and
   dependencies.
3. Then hit the “Generate Project” button. This will begin the hello-rest.zip file
   download.
   On completion of the download, extract the contents of the zip file. You will see the hello-rest folder
   generated.
4. The next step is to launch and run our application. To do this, open a command line, navigate to the
   hello-rest folder, and run the following command:
   mvn spring-boot:run
5. To test our running application, launch a browser and navigate to http://localhost:8080/greet


Spring Initializer
-
The Spring Initializr application hosted at http://start.spring.io itself is built using Spring Boot. 

Generating a Project using STS
-
Spring Tool Suite or STS is a free Eclipse-based development environment that provides great tooling
support for developing Spring-based applications. You can download and install the latest version of STS
from Pivotal’s website at: https://spring.io/tools/sts/all. 

1. Launch STS if you haven’t already done so. Go to File ➤ New and click on Spring
   Starter Project
2. In following screen, enter the information. In addition to
   entering Maven’s GAV information, select the Web starter option. Hit Next.
3. On the following screen, change the location where you would like to store
   the project. The “Full Url” area shows the HTTP REST endpoint along with the
   options that you selected 
4. Hit the Finish button. You will see the new project created in STS


Generating a Project Using the CLI
-
Spring Boot provides a command line interface (CLI) for generating projects, prototyping, and running
Groovy scripts.

1. Download the latest version of the CLI ZIP distribution from Spring’s website at
   http://repo.spring.io/release/org/springframework/boot/spring-boot-cli.
   At the time of writing this book, the current version of CLI was 1.2.1. This version
   can be downloaded directly from http://repo.spring.io/release/org/
   springframework/boot/spring-boot-cli/1.2.1.RELEASE/spring-bootcli-1.2.1.RELEASE-bin.zip.
2. Extract the zip file and place its contents (folders such as bin and lib) under
   C:\tools\springbootcli 
3. Add a new environment variable SPRING_HOME with value c:\tools\
      springbootcli.
4. Edit the Path environment variable and add the %SPRING_HOME%/bin value to
   its end.
5. Open a new command line and verify the installation running the following
   command:
   spring --version
6. Now that we have the Boot CLI installed, generating a new project simply involves running the
   following command at the command line:
   spring init --dependencies web rest-cli
   The command creates a new rest-cli project with Web capability


Postman
-
Postman is a Chrome browser extension for making HTTP requests. It offers a plethora of features that
makes it easy to develop, test, and document a REST API.

To install Postman, simply launch the Chrome browser and navigate to https://chrome.google.com/webstore/detail/postmanrest-client/fdmmgilgnpjigdojojpjoooidkmcomcm.


RESTClient
-

RESTClient is a Firefox extension for accessing REST APIs and applications. Unlike Postman, RESTClient
doesn’t have a lot of bells and whistles, but it provides basic functionality to quickly test a REST API. To
install RESTClient, launch the Firefox browser and navigate to the URL https://addons.mozilla.org/enUS/firefox/addon/restclient/. Then click the “+ Add to Firefox” button and in the following
“Software Installation” dialog click the “Install Now” button




# Rest APIs

What is an API?
-
An API is the bridge between the website and the back system's information without having to manually entering it all at once.
Requesting data

What is HTTP?
-
HTTP provides a standardized way for applications to communicate with each other over the internet. 
HTTP defines a set of request methods, response codes, headers, and message formats that are used to exchange data between clients and servers.

Message formats
-
HTTP messages are the way that clients and servers communicate with each other. 
There are two types of HTTP messages: requests and responses.

- Request:
  - Request line: This includes the HTTP method being used, the URL of the resource being requested, and the version of the HTTP protocol being used.
  - Headers: These are additional pieces of information sent along with the request that provide more context about the request
  - Body: This is an optional part of the message that contains data being sent along with the request, such as form data or JSON.
- Response:
  - Status line: This includes the HTTP version being used, a response code indicating whether the request was successful or not, and a short message explaining the response code.
  - Headers: These are additional pieces of information sent along with the response, as explained in my previous answer.
  - Body: This is an optional part of the message that contains the data being sent back to the client, such as HTML or JSON.


 HTTP Methods
-

CONNECT
This method is used to establish a network connection to a resource. When a client sends a CONNECT request, the server responds with a tunnel that can be used to establish a secure connection to the specified resource.

TRACE
This method is used to retrieve a diagnostic trace of the communication between a client and a server. When a client sends a TRACE request, the server responds with a message that contains a copy of the request and response headers.

Key features of REST APIs
-
REST APIs allow applications to interact with each other over the internet by sending and receiving data in a standardized way. 
    
- Client-server architecture
  - A client-server architecture, where the client (such as a mobile app or web application) interacts with the server (which provides data and resources) via HTTP requests and responses.
- Stateless communication
  - Stateless communication, where each HTTP request contains all the necessary information for the server to understand and fulfill the request.
- Uniform interface
  - A uniform interface provides a standardized way for the client to interact with the server. They use a common data format, usually JSON or XML, to exchange information. 
- Use of hypermedia
  - The use of hypermedia, where the response from the server includes links to related resources, allows the client to navigate through the API.

Best Practices for REST API
-

Use meaningful resource URIs
Use resource URIs that are easy to understand and identify. A resource URI should clearly indicate what the resource is and what it represents.

Use HTTP methods correctly
HTTP methods like GET, POST, PUT, and DELETE should be used correctly to perform the appropriate action on the resource. 

Use nouns instead of verbs in URIs
Use nouns instead of verbs in URIs to represent the resources. This approach makes the API more intuitive and easier to use. For example, instead of using /createUser, use /users.

Use consistent naming conventions

Provide comprehensive documentation

Versioning

Use proper security measures

Consider performance

Handle errors gracefully

Building a REST API
-

1. Identify the resources
2. Choose the appropriate HTTP methods
3. Design the data model
4. Choose a framework
5. Implement the endpoints
6. Test the API
7. Document the API
8. Deploy the API


Building A RESTful Web Service
Key Points:
What the Service Does:

The service responds with a greeting, such as {"id":1,"content":"Hello, World!"}.
An optional name query parameter can be included (e.g., /greeting?name=User), and the service will return {"id":1,"content":"Hello, User!"}.
Tools Needed:

Java 17 or later.
Gradle 7.5+ or Maven 3.5+.
A text editor or IDE like Spring Tool Suite, IntelliJ IDEA, or VSCode.
Setup:

You can start from scratch using Spring Initializr to generate a project, or clone the project from GitHub.
Steps to Build the Service:

Create a Resource Representation: Define a Greeting class with id and content fields that represent the response data.
Create a Resource Controller: Use @RestController to handle GET requests at /greeting. The controller formats the greeting message and returns a Greeting object.
Application Setup: The @SpringBootApplication annotation sets up the application, and Spring Boot’s auto-configuration handles dependencies and infrastructure.
Running the Service:

You can run the service using Gradle or Maven. The service can be packaged as an executable JAR file, which simplifies deployment.
To test the service, visit http://localhost:8080/greeting, and it will return the greeting message in JSON format. The name query parameter can modify the greeting message.