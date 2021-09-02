- **Chapter 1: BASH**
  - 1.1
    - **CLI** uses terminal emulators. Emulate behavior of OG terminals
    - REPL = read evaluate print and loop environment
    - Bash examples denoted with a `$ command` and powershell denoted with a `> command`
  - 1.2 shell fundamentals
    - relative paths: directions relative to current directory as starting point
    - absolute paths: directions from fixed/absolute starting point
      - starts from a root directory
    - This chapter seems a bit wordy to be honest
    - windows root directory is `C:`\
    - linux it is simple `/`
    - Calling commands
      - format: `$ program <arguments(s)> [--option]`
        - this follows the format of *program*, *positional arguments* and then *options/modifiers*
        - arguments must be provided in a specific order
      - Example command:
        - `$ dotnet new webapp --name MyApp`
          - **program**: `dotnet`
          - **first argument**: `new` (the argument for creating new projects)
          - **second argument**: `webapp` (a sub-argument of `new` for defining what type of project to create)
          - **option**: `--name` (option to define the name of the new project)
          - **option argument**: `MyApp` (the value for the `name` option)
        - shell managers. will not be using language managers likes npm or pip (though samy terms apply)
    - windows packages are handled on chocolatey package manager or `choco`
      - https://chocolatey.org/
    - on OSX it is homebrew
      - https://brew.sh/
    - For linux we will use apt as it is the default package manager on Debian based distros (like ubuntu)
    - the tools installed with package managers are stored/hosted in package repositories on the web/
      - package managers come with some default repo packages from trusted package maintainers
    - Tools used on class:
      - `dotnet`
        - used to make, manage and run .NET projects
      - `az`
        - the azure cli for provisioning + managing cloud resources
      - `git` 
        - our version control cli
    - Shell environment variables
      - shell environement holds environment variables that configure aspects of a shells behavior
      - HOME variable
        - this tells us what our home directory path is. it is `$HOME` in linux and `$Env:HOMEPATH` in powershell and our shell will navigate to this path when starting up
      - ECHO
        - can view variables with this command. like `echo $Env:HOMEPATH` in powershell
          - will output home variable like: `C:\Users\YourUsername`
      - PATH variable
        - holds collection of base paths that shell should look for when evaluating a command
          - `$PATH` in bash
          - `Env:Path` in powershell
          - example of a PATH variable with 4 base directories to look in for a program:
            `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin`
            - if you were to type `ls`, it would search right to left in the paths listed above for a file named `ls`
    - Piping
      - process of chaining multiple commands together using the output of the prior command as input for the other
        - specified with the `|` character
        - ex:`$ first-command | next-command <first-command output>`
  - **1.3 Walkthrough: linux + bash setup with WSL**
    - viewing available distros
      - `wsl --list`
      - `wsl --list --running`
        - included all running VM's
    - Entering a vm shell directly
      - `wsl --distribution Ubuntu-18.04`
    - shutting down a vm
      - `wsl --terminate Ubuntu-18.04`

------

***Chapter 2 Bash Fundamentals***

+ file paths are separated by forward slashes

+ all inputs and outputs in bash are strings of chracters

+ getting helpl

  + `command --help`
  + `man help` 

+ show CWD

  + `pwd`

+ go to home dir

  + `cd ~`
    + ~ is shorthand for the home directory of the user currently logged in

+ **2.3 Directory operations**

  + `mkdir some_dir`

  + move directory

    + `mv <path to target> <path to destination>`
    + `mv /tmp/child-dir parent-dir/child-dir`

  + copy directory recursively

    + `cp -r parent-dir /tmp/parent-dir`
      + copies files of parent-dir to the /tmp/parent-dir

  + deleting a directory

    + DO NOT take this likely. shell deletion is permanent and near instantaneous. always use absolute path to be explicit and precent mistakes

    + ```bash
      rm -i -r /tmp/parent-dir
      ```

      + removes the parent dir recursively with the interactive mode for a safety measure 

  + create foile

    + `touch new-file`

  + view file conents

    + `cat new-file`
    + `less path/to/file`
      + more helpful to scroll through

+ **2.4 Bash scripting fundamentals**

  + it is customary to use `.sh` file extension as a note to signify that the script is to be interpreted as bash commands
  + comments
    + use #
      + comments are used to dictate the *why* of commands, not the *how*. that's what the code is supposed to do
  + REPL is interpreter implied by bash
    + example execution
      + `bash ~/my-script.sh       # this will execute my-script.sh in the home directory`
    + implicit execution is when interpreter is defined inside the script using a *shebang (#!)*

+ **2.5 Substitutions and Scripting in Bash**

  + substitution is when a value is well... substituted into another expression.

  + ***script variables are scoped to the script***

  + ***environment vairables are globally scoped to the whole shell***

  + variables

    + convention is to use lowercase letters and separate words with underscores (_).
    + environment var's are written in all capital letters
    + variables declared with =
      + THERE CAN BE NO SPACES BETWEEN VARIABLE DECLARATION AND ASSIGNMENT
        + correct example:
          + `variable_name=value`
        + when need spaces, put single quotes around value
          + `variable_name='a longer value'`
    + spaces are user to distinguish different parts of a command (called a token). token splitting is what allows bash to see a command along with its arguments and options to be evaluated
    + referencing variables
      + `$my_variable    #local variable`
      + `$PATH    #global variable`

    + quoting syntax

      + when referencing variables in commands, use "" as it is best practice. Prevents unintended behavior caused by spaces of special characters
        + Ex:) `mv "$target_path" "$destination_path"`

    + Command substitution

      + like variable substitution but for commands. allows to execute a command with another command. also called **in-line executions** 

      + ex:) 

        + `command $(sub-command)`
          + in this example, sub command will be evaluated first (in-line), then the main "command" will be evaluated. the output from sub-command will be substituted in as it's argument

      + in-line commands will be evaluated first (like with all programming languages, inside out)

        + ex:)

          + ```bash
            $ command $(sub-command $(sub-sub-command))
            ```
            + sub-sub command done first, then sub-command, then command

+ **2.6 walkthourgh, working with a package manager in bash**

  + ubuntu comes installed with apt (advanced packaging tool)

    + ```bash
      apt <action argument> -y
      ```

      + the -y option just skips confirmation prompts to speed up the process, that is all

  + will need to use sudo with apt often, as privilege's are often outside domain of user's permissions

    + stands for **substitute user to DO the command to the right of it** 
      + If we don't specify which user to use (which often dont) it will default to root user

  + updating repo sources

    + `sudo apt update`
      + will download metadata about installed packages like pending upgrades. but will *not* install the upgrades. need to do `sudo apt upgrade` to do that

  + installing cli tools

    + `sudo apt search <search term>`	
      + will search/scan source repositories for a package. If search results contains a package, you can install it with the `install` argument
    + `sudo apt install <package name> -y`
      + installing a package

  + removing a package

    + `sudo apt remove cowsay -y`
      + this will remove the cowsay package

  + adding custom sources

    + require usually *signing key* and the *repository* to be installed. example: installing dotnet CLI
    + first step. install official microsoft package repo. includes both the repo AND the signing key
      + ` sudo wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb`
        + installs package and signing key
      + `sudo dpkg -i packages-microsoft-prod.deb`
        + unpacks and installs the repo + trust the signing key

  

------

**Chapter 3 Web APIS**

+ 3.1 web apis

  + a way to think about web apis are mvc with the v (they are not concerned with the presentation of data)

  + a web api is ***consumed by a client application***, this involves transferring data instructios and representations like CRUD operations

  + chief responsibility of a web api: provide interactions with and exchange representations of data with its client application

  +  hypothetical steps of a web api using an ORM

    1. A request from a client application for is received by the application.
    2. The appropriate controller method is called to process the request.
    3. The controller determines if the request is valid.
    4. The controller retrieves data using an ORM system.
    5. The controller transforms the object into a JSON representation.
    6. The controller responds to the client with the JSON representation.

  + web api data will be passed and parsed back to the client with JSON

  + As a primer, recall that HTTP:

    - Is a stateless request/response protocol.
    - Requests and responses *may* include HTTP bodies but are not required to.
    - Responses always contain a three-digit HTTP status code.
    - Requests and responses *always* include HTTP headers.

  + MIME-type: associated with the HTTP header `Content-type`

    + header for html documents:

    + ```html
      <!DOCTYPE html>
      <html>
         <head>
            <title>My Web Page</title>
            content
         </head>
         <body>
            content
         </body>
      </html>
      ```

- Http status codes

  - https://education.launchcode.org/azure/chapters/web-apis/intro_web-apis.html

- **3.2 REST Abstract Fundamentls**

  - State - refers to transitional data that can be viewed or changed by external interaction
    - you can interact with state using crud
      - before creating: empty state
      - after creating: initial state
      - after updating: new state
      - after deleting: empty state
  - Representaiton - depiction of state that is usable in a given context
    - JSON is the standard representation used when transferring state between a client application and an api
    - object representation to JSON representaiton = **JSON serialization**
    - JSON to object representation = **JSON deserialization**
  - Transferring representation of state
    - client is in control of the state, means the client can do CRUD operations
    - It is up to the apito define the contract/expose
      - type of state/resources that client can interact with
      - which crud interactions are supposed for each resource
  - Resources 
    - resource is representation of a specific type of state
    - resources categorizd as individual entity or a collection:
      - **entity:** single resource uniqiely id'd in a collection
      - **collection:** entities of same resource type treated as a whole

- **3.3 REST: pracitcal fundamentals**

  - 3.2 overview:

    - **State**: data that can change, or transition, through interactions between an API and its client.
    - **Representation**: the convertible format that enables state to be transferred and used by the client and API.
    - **Resource**: the representation of a type of state (as an entity or collection) that the API exposes to its client for interaction.
    - **Entity**: a single resource that is uniquely identifiable in a collection.
    - **Collection**: entities of the same resource type treated as a whole

  - Shapes

    - shape = blueprint that describes representation of an input or output of an api

      - think like an object oriented application

        - blueprint:

          - ```json
            CodingEvent {
               Id: integer
               Title: string
               Description: string
               Date: string (ISO 8601 date format)
            }
            ```

        - JSON representation:

          - ```json
            {
               "Id": 1,
               "Title": "Halloween Hackathon!",
               "Description": "A gathering of nerdy ghouls to work on GitHub Hacktoberfest contributions",
               "Date": "2020-10-31"
            }
            ```

      - directions step by step
      
    - Q: So how do we use this JSON data that's supposed to be the representation of shape?
    
      - Q: Have we gone over parsing?
    
    - Q: API is responsibe for managing things like a unique identifier, (such as Id), timestamp and links for relationships between resources. Are there any other common things that an API is normally responsible for?
    
  - Endpoints

    - consists of http path and method that defines location of a resource and the action to take on its state
    - an api exposes endpoints to its consumers, each endpoint is made up of a:
      - path: identifies the resource
      - method: action to take on the resources state
    - RESTful api's separate resources they expose into one or more resource entry-points. 
      - these entry-points identify each top-level resource collection
    - entry points are pluralized
    - example of request to endpoint
      - ![image-20210825111237421](C:\Users\Blake\AppData\Roaming\Typora\typora-user-images\image-20210825111237421.png)

  - Crud operations and HTTP Methods:

    - ![image-20210825111319762](C:\Users\Blake\AppData\Roaming\Typora\typora-user-images\image-20210825111319762.png)

- Status Codes:

  - ![image-20210825111749042](C:\Users\Blake\AppData\Roaming\Typora\typora-user-images\image-20210825111749042.png)
  - ![image-20210825111819203](C:\Users\Blake\AppData\Roaming\Typora\typora-user-images\image-20210825111819203.png)

- Headers

  - headers are used to communicate metadata about each interaction with a resource

- **3.4 Walkthrough**

  - in windows we need to install Chocolately to installed third-party CLI tools
  - Q: in 3.4.1 we tell them to install chocolately, however, I'm pretty sure we already did that in chapter 1
  - Adding a chocolatey source:
    - `> choco add source <source target>`
  - Q: this is more of a statement, but it seems very odd to me about the walkthrough and then jumping straight into the studio
  - Q: it says to clone, do we want to fork this repo first though?
  - Q: pictures in 3.5.3.2 do not line up with postman options
  - As a best practice, we explicitly define the `Content-Type` header. This header indicates that our request contains `application/json` data so that the API knows how to parse the incoming request body

------

**Chapter 4 Web Hosting**

+ **4.1 Hosting**
  + publishing
    + `dotnet publish`
      + loads all the files and everything we have loaded as dependencies into out project
      + files created by our source code are known as build artifacts
    + steps to running a project on a server:
      1. Publish a project
      2. Transfer the published artifacts to remote server
      3. Download the `dotnet` runtime SDK on remote server
      4. Run artifacts using the dotnet runtime SDK
  + cloud hosting
    + these 3 issues are solved by cloud hosting:
      1. Networking
      2. Discovery 
      3. Availability
    + we will do this with azure, and will be able to copy over the build artifacts created with dotnet publish and then run our application
+ **4.2 Networking**
  + 2 types of networks
    1. Private intranets
    2. public internet
  + IP Addresses
    + unique id that id's each computer or device connected to a network
    + we will be using ipv4 addresses in this class
    + we have worked with a loopback address already
      + `127.0.0.1`
        + when requests are made to this address, the request is sent back to the machine that made the request. this is `localhost`
  + LAN's
    + private network that connects all copmuters/devices in a small geographic region. 
    + Think like our house lan, connecting printer, smart phone, computer, etc. and anything else onto one network.
      + could also be things like offices, hotels, convention center, restaurant
  + Private Ip's
    + ip address that id's a device within a lan.
      + router is responsible for providing private ip addresses to each computer or device.
      + LAN gives comp/device ability to communicate with each other but LAN does not have access to internet (yet). router must be connected via an ISP. the ISP assigns a public IP to your router and this address is used when a computer/device makes a request to the internet
  + WAN's
    + collection of connected private networks in large geographic region.
    + divies out unique private ip's to devices on the network.
    + WAN often uses public infrastructure to establish a connection between two or more private LAN's. like a city, state, country, public transit, big org with several buildings.
  + Internet
    + collect of inter-connected public networkins. Think of it as avery large, publicly accessible WAN. Must have an ISP assign a public IP address so you can access other computers/servers on internet.
  + Public ip's
    + ip that id's end-users + servers on greater internet.
      + end-users are people that access internet. end users and machines need to have unique Ip's
  + DNS - naming system for IP addresses and domain names. Simplifies so we don't have to type in publicly accessible IP
    + ex:) enter google.com. computer uses a DNS to resolve it to an IP of `88.31.122.3` and then that address is used to make the request to a specific machine where the web page/ data is hosted
+ **4.3 Processes and Ports**
  + process = actively running istance of a program
    + will always have PID + some info about which application process associated with.
  + Ports
    + just simply put. a communication endpoint.
    + way to  determine which specific application to access on a remote computer.
    + need to know two things to access an application.
      1. IP address of computer/server
      2. port it is being run on
+ **4.4.**
  + Q: says need a parterner (mentioned in 4.4.2)
  + `dotnet public -c Release`
    + will publish to `**CodingEventsAPI/bin/Release/netcoreapp3.1/linux-x64/publish/**` 
  + Q: says "This will publish to `CodingEventsAPI/bin/Release/netcoreapp3.1/linux-x64/publish/`" - 4.4.2.1
    + BUT, linux-x64/ folder is nowhere to be found
    + oh crap, it's supposed to be on linux, but doesn't say this anywhere

------

**Chapter 5 Azure and VM's**

+ **Intro to infrastructure and IAAS**
  + they lease access to infrastructure services hosted in data centers
    + data centers are large buildings that contain physical infrastructure like computer, networking equipment, storage disks, etc.
  + The process of subdividing servers, storage and networking is *virtualization*
    + this is what you rent 
  + Part of *why* you rent from a CSP is a guarantee of high availability/server uptime.
    + As well as not having to deal with the hardware
  + Infrastructure 5.1.2
    + infrastrucure just used as a reference to hardware + software used to dpeloy code
    + IaaS = CSP renting access to virtualized service through a digital interface
      + most CSP's give a web UI and a CLI tool to manage, configure and provision(create) the infrastructure rented
    + CSP's usually rent out
      + disk storage
      + computation
      + networking
  + Disk Storage
    + Scaling = process of increasing/decreasing resources based on demand
  + Computation
    + 2 types of scaling for a CSP:
      1. Vertical
         - increasing/decreasing power of CPU cores and RAM on single machine
      2. Horizontal
         - increasing/decreasing number of VM's with same amount of CPU + RAM
  + Azure for each subject area within CSP:
    + Compute: [Azure Virtual Machine](https://azure.microsoft.com/en-us/services/virtual-machines/)
    + Storage: [Azure SQL Databases](https://azure.microsoft.com/en-us/services/sql-database/) & [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/)
    + Network: [Azure Virtual Network](https://azure.microsoft.com/en-us/services/virtual-network/)
+ **5.2 intro to azure**
  + Key vaault = accessible service that stores info such as sensitive or configuration data. Like a database connectio string.
  + azure AD (azure active directory) = allows to authenticate users. Will use OAuth in this book. But azure does support others types of authentication. Also offers SQL services
  + Azure resource groups = organizational tools to help manage azure resources. Create resource groups per project
+ **5.3 Intro to VM's**
  + Vm = computer file (referred to as image) that behaves like actual computer. 
  + ***<u>MAKE SURE TO STOP VM'S WHEN DONE IN AZURe</u>***
  + parity = equivalence between a development environment and the production environment.
+ **5.4 Walkhrough**
  - Steps for deployment with azure:
    1. Create a resource group
    2. Spin up a VM with an Ubuntu OS
    3. Configure our VM to run our application
    4. Create our source code
    5. Create the build artifacts (publishing)
    6. Deploy the build artifacts
    7. Open the VM security group
    8. Connect to deployed project	
  - Q:A Make SURE students enters the username and password for their server correctly when creating it. (5.4.3)
  - Q:A Password and username are listed in 5.4.3 for this server https://education.launchcode.org/azure/chapters/azure-intro/walkthrough.html
  - windows users might have to use the publish folder.

------

**Chapter 6 Secretes Management and Backing Services**

- **6.1 Secrets Management**
  - secret = privileged piece of info that our application needs to operate. LIke a database string
    - way to protect senstitive data like PII
      - PII/sensitive data usually secured with a data backing service
  - Secrets are also kept separate from the source code of the application
    - examples:
      - Databases connection strings
      - API keys
      - Usernames
      - Passwords
      - VM IP addresses
  - Handling sensitive data
    - keep it out of version control
    - app secrets are stored externally and loaded at runtime
    - infrastructure must grant least-privileged access
    - PII/proprietary data securely housed and requires authorization
  - BE CAREFUL of what files we track with Git.
    - never stage and commit it to a git repo.
    - this is tracked by gitignore
      - Q:A Wth? We already went over gitignore files before
  - External Configuration
    - best practice for handling secrets is to use `external configuration` files. 
      - keeps our config files outside of our project file tree
        - examples of this would be like the `appsettings.json` file.
    - these files have 2 main advantages:
      - Secrets are kept separate and safe from both the code and running application
      - An application can have different configuration for different environments, such as different database connection strings for staging and production
  - Secrets Management
    - We will use azure key vault
    - `dotnet user-secrets` is a module in the dotnet CLI. this is a local secrets manager
      - it creates a secrets store on our machine. when initialized for a project, our project configuration file (.csproj) will automatically be updated with its ID
- *Skipped through 6.1 and 6.2 just to get a brief overview, so I didn't include super detailed notes here
- **6.2 Application Environments**
  - `Local->Development->Production` is the typical environments during a normal development cycle
    - Testing and staging are here too but they're outside the scope of this classs
- **6.3 Backing Services**
  - any service a MVC app needs is a backing service
  - Examples:
    - relational db's (MYSQL)
    - Non relation Dbs (MongoDB)
    - Cachine services (Redis, Azure caching, Memcached)
  - External Backing services
    - a service that runs externally to the application. i.e. they run on different host machines and commuincate over an external network
      - like a managed database
  - Internal Backing Services
    - services ran on the same machine as running applicaiton that needs it
      - this is what we have used in class so far. local environments usually do this
  - App Environment best pracitces
    - local
      - use internal backing services
    - development
      - *should* use internal backing services. 
      - supposed to be fully automated and only generate reports. CI system should be handling most of this
      - but this is also dependent on the nature of the project as well, internal may not *always* be best solution
    - production
      - use external backing services. use a CSP
      - 
- **6.4 Walkthrough**
  - fork the repository, clone the repository
  - this walkthrough is heady. Aggressive. Set in a different format from the other walkthrough
    - tries to teach half of it while also walking through it.
  - in 6.4.2 it gets better here, becomes much more straighforward
  - 6.4.11 throws an error:
    - ![image-20210827160308245](C:\Users\Blake\AppData\Roaming\Typora\typora-user-images\image-20210827160308245.png)

------
**Chapter 7 Authentication and Authoriation with Azure**
- **7.1 Introduction and key concepts**
  - will learn 2 security protocols
    1. `OAuth`
    2.  `OIDC`
  - `AADB2C` - Azure active directory business to customer
    - used to securely manage interactions between applications and user accounts
  - `Authentication` - process used by an entity to prove their identity
    - think logging into your user account.
  - `Authorization` - permissions needed to access a protected resource
    - only entities authorized to access a resource are allowed to do so
    - ` access control systems` define rules (aka `policies`) for managing authorization
  - Server roles
    - resources are managed by a `resource server`. Like an email provider
    - will use AADB2C as an `authorization server`to protect our api from unauthorized requests.
  - Delegation
    - `delegation` - authorization for an entity to act on behalf of another
  - OAuth and OIDC
    - a user delegates authorization to a client through the use of a digital token
    - this client uses the access token to prove that they are authorized to access a resource according to permission granted
  - OIDC
    - is built over OAuth
    - is used to delegate authentication through use of an `identity token`. 
      - `identity tokens` prove the identity of the user. Is what enables SSO's (`single sign on's`) on the web
        - i.e what allows you to log in to many different client services using a single identity token

- **7.2 Walkthrough: OAuth and OIDC**
  - both OAuth and OIDC specs (Specificatinos) are designed for a user to securely coordinate an exchange of info between two unrelated services
    - `client service` - requires a user to authorize access to their identy or data
    - `provider service` - provides said identity or data access of a user who authorized a client services.
  - Again
    - OAuth - used for authorizing access of users data on provider service to client service
    - OIDC - id provider service used to rovide ideneity of a user in order to authenticate with a client service
  - Q/A: uh.. are we supposed to be doing this in ubuntu or powershell? because it's not made very clear here. Also. In the readme for the Visual OAuth repo, it gives this command: `npm run setup:windows`. But it never told us to install npm from what I recall
    - also did not tell us to generate a new client secret either
  - Have to **absolutely** make sure to use the windows instructions when doing this walkthrough
  - OAuth grant flow:
    - This grant flow results in an access token. 
    - This access token authorizes the client API. 
    - This allows client API to access a user's github data
  - OAuth `implicit grant flow`
    - Q/A: Why the heck was this brought up in a walkthrough?
    - Q/A: you're *supposed* to run through the application that loads? That was nowhere in the instructions. Had to infer that from what the instructions we're "reviewing" what we did.
    - anyways, onto the grant flow:
      1. User Authenticates with the Provider and consents (Authorizes) the Client to access data on their behalf
      2. Provider Redirects to Client with Auth Code
      3. Client Front-end Sends Auth Code to Back-end
      4. Client Back-end Exchanges (by using it’s client secret) Auth Code For access token
    - Q/A: 7.2 walkthrough needs a re-write. Give theory first, then instruction.
    - two types of tokens
      - `access tokens` - used as proof of authorization for a client wanting to access resources
      - `identity tokens` - used to share identity to authenticate
    - JWT's  (java web tokens)
      - `JWT` - way of securely transferring payload data that can be verified. 3 componenets
        1. `header`: jwt's metadata
        2. `payload`: JSON data
        3. `signature`: digital signature (for authenticity)
    - Brain melted into floor, after seeing identity/access tokens re-defined.. again?
  

