
You don't need to memorize most of these things, if you need to use use them you can just come here and see how to do that. Focus on understanding things.

# Dotnet Execution Process
##### **NON-DOT NET** **Program Execution** **Process**:

Language -> compiler -> machine language code.

platform dependent
##### Dotnet execution Process:

.NET supported language --.NET supported language compiler--> Managed code --(at runtime)JIT compiler--> machine language code

platform independent.
##### Final Considerations:
Languages that are not supported by .NET will not enjoy the CLR services like the garbage collector for example. Therefore in languages like C you the developer has to do the memory deallocation manually.

None of them store the native code. In both cases the native code is always thrown away when the program stops running.

Unmanaged resources: resources that were written in languages that are not supported by the .NET framework cannot enjoy the CLR services and need to be manually de-alocated from memory.


# Intermediate Language Code

Assemblies (.exe or .dll) consist of IL code and Manifest code.

The Manifest contains metadata about the assembly like the name of the assembly, the version number of the assembly, culture, and strong name information.

Metadata also contains information about the referenced assemblies. Each reference includes the dependent assembly’s name, assembly metadata (version, culture, operating system, and so on), and public-private key if the assembly is strongly named.

The .NET framework provides a nice tool called **ILDASM (Intermediate Language DisAssembler)** to view the code of the intermediate language in .NET Framework.

open Developer Command Prompt for Visual Studio 2022 with administrator privildeges and type:
**Ildasm.exe filepath.exe/.dll**


## How to Change Assembly Information

Using ILDASM tool go to `Properties`->` AssemblyInfo.cs`

to change version: [assembly: AssemblyVersion("2.0.0.0")]


## How to Turn IL code into a Text File and Rebuild that Assembly using ILASM

first using ILDASM dump the IL code with Dump Options and select: Show Progress Bar, Dump IL Code, Expand try/catch select (only these 3) to a text file.

to rebuild in VS Command Prompt type:  **ILASM.exe D:\MyFile.il


# Managed and Unmanaged Code

## Managed Code:

resume: code that is written in .NET supported languages. Because they are capable of enjoying the services provided by the CLR.

Whenever we create any EXE (i.e. Console Application, Windows Application, Class Library Project, etc.) or any DLL i.e. Web Application (i.e. ASP.NET MVC, ASP.NET Web API, ASP.NET, etc.) in .NET Framework using Visual Studio and using any .NET supported Programming Language such as C#, VB, F#, J#, etc., then these applications are run completely under the control of CLR (Common Language Runtime). Therefore it is code that is managed by the .NET Framework.

If your applications have unused objects, then CLR will clean those objects using [**Garbage Collector**](https://dotnettutorials.net/lesson/garbage-collector/). If your application wants to communicate with other applications, then it will make sure that CTS ([**Common Type System**](https://dotnettutorials.net/lesson/common-type-system/)) and CLS ([**Common Language Specifications**](https://dotnettutorials.net/lesson/common-language-specification/)) are followed. CLR also uses **CAS (Code Access Security)** and **CV (Code Verification),** to manage the security of your application i.e. check whether the current user has rights to access the assembly or not and whether it is safe to execute the assembly by the Operating System or not. The CLR will also load your application and unload your application, etc.

Managed code will enjoy of all of the services provide by the CLR.

The main disadvantage of managed code in the .NET Framework is that we are not allowed to allocate memory directly, or we cannot get low-level access to the CPU architecture. This is going to be managed by the CLR of the .NET Framework.

This takes away reduces the amount of control a developer has over it's own program.

## Unmanaged Code

resume: code/resources/apps/program written in not .NET supported languages. Therefore cannot enjoy CLR services like garbage collection which results in you having to manually dealocated unmanaged objects from heap memory for example.

Third-party EXE in your .NET application like Skype, PowerPoint, Microsoft Excel, Whatsapp, etc. These “EXEs” are not made in dot net, they are made using other programming languages let us as C and C++.

They are assemblies that are not run in the .NET Framework runtime environment (CLR). They run in their on separate environments

The CLR cannot provide any of it's services to these assemblies. They will be compiled straight to native code for example. They might also require manual garbage collection.

##### **What are the advantages of using Unmanaged Code?**

1. It provides the low-level access to the programmer.
2. It also provides direct access to the hardware of the machine.
3. The code is a little bit faster than managed code.
4. We can run the Unmanaged Code under any environment and platform.

##### **What are the disadvantages of Unmanaged Code?**

1. It does not provide security to the application.
2. Due to the access to memory allocation, issues related to memory leakage might have occurred.
3. No automatic garbage collection.
4. It will not support default exception handling, the developer needs to take care of that things.

# Assemblies

Assemblies are the.NET Framework unit of the deployment. They are also just precompiled .NET code that can be run by CLR.

Assemblies are created when you precompile source code, configurations and other assemblies. This precompiling to IL code is what makes .NET Framework supported languages so fast compared to other high-level languages, the reason why is because IL code is very low-level aka very close to machine language code/binary code that can be understood by computers at the hardware level.


1. **EXE (Executable)**: this assembly has it's own memory space and it's own address. If you run the same .exe assembly at the same time, they will have their each unique and separate memory space. It is independent, in the sense that it can run by itself.
2. **DLL (Dynamic Link Library)**: cannot be invoked or run by itself. It needs a consumer who is going to invoke it. So, a DLL is run inside other memory space. Dependent on executable assemblies (like the IIS module for Web Applications (which are .dll assemblies)). The point of .dll is reusability, it si meant for you to implement classes, logic, etc that you want to use in many applications.


# Strong and Weak Assemblies

In .NET Framework Base Class Library, we have several assemblies. All the .NET Framework assemblies are installed in a special location called GAC (Global Assembly Cache).

Starting with the .NET Framework 4, the default location for the Global Assembly Cache is **%windir%\Microsoft.NET\assembly**

To see the installed assemblies we need to open Visual Studio Command Prompt and we need to type **gacutil -l** or **gacutil /l** command

All the assemblies present in GAC are strongly typed. If you try to install a weakly typed assembly it will thrown an error.

In .NET Framework, an assembly consists of 4 Parts. They are as follows:

1. Simple Textual Name (i.e. Assembly Name).
2. The Version Number.
3. Cultural information (If provided, otherwise the assembly is language-neutral)
4. Public Key Token

Weak assemblies have a Private/Public Key Pair and Strong Assemblies do not.

If an assembly is not signed with the private/public key pair then the assembly is said to be a weak named assembly and it is not guaranteed to be unique and may cause the DLL Hell Problem. The Strong Named Assemblies are guaranteed to be unique and solve the DLL Hell Problem. Again, you cannot install an assembly into GAC unless the assembly is strongly named.


### Explanation of the 4 Parts of an Assembly Manifest


#### Name

This is nothing but the project name. We have created one console application with the name **AssemblyDemo**. Now build the project and go to the **Bin => Debug** folder of your project and you should find an assembly with the name **AssemblyDemo.**
#### **Version Number:**

The default format of the Version number is **1.0.0.0**. That means the version number again consists of four parts as follows:

`FileVersion`:
1. **Major Version**
2. **Minor Version**
3. **Revision Number**
4. **Build Number**

Here is a [Cheatsheet](https://gist.github.com/jonlabelle/34993ee032c26420a0943b1c9d106cdc)

To change the assembly version `AssemblyInfo.cs` implement: **[assembly: AssemblyVersion(“a,b,c,d”)]** (a-d are numbers/a=Major/d=Revision).

`AssemblyInfo.cs` is just another regular `.cs` file. The only difference is that it is used for the only purpose of implementing/writing [assembly: someMethodOrWhatever] attributes. This attributes could be implemented in any other `.cs` file and have any other name. But you should follow convention's as much as possible.

##### You don't need to create a `AssemblyInfo.cs` anymore to change the version:
So the `AssemblyInfo.cs` will be redundant for the most part. Nowadays you can change the `AssemblyVersion`, `FileVersion` and `Version` through the `<PropertyGroup>` Element of a Project by opening a project in `VisualStudio`  and double clicking on the project (not on the project's file's).

##### Difference Between `Version`, `FileVersion` and `AssemblyVersion`:
`AssemblyVersion` is only really used when being loaded by the CLR (I think), as I believe it is part of the assembly's 'signature', if you will. This version is important when assembly binding. Because of how we distribute our products and libraries, we generally only ever increment the `<major>` component of the version. So if the app's version is something like 2.3.7, our `AssemblyVersion` will be 2.0.0. If we changed this version each time we did an update, you'll find yourself running in assembly binding exceptions often. Since I don't mess around with this version much, others might have a better idea.

`FileVersion` is the version of the file itself and, as far as I am aware, is primarily used by installer software, especially those that are MSI-based. For our purposes, we keep the `FileVersion` and `Version` the same, ignoring any pre-release notations that you'll find in the `Version` due to semantic versioning. So, if the `Version` is 2.3.7+alpha.8, then the `FileVersion` is 2.3.7.0 or 2.3.7.

`Version` is equivalent to the old `[AssemblyInformationalVersion]` attribute and can technically be anything (i.e `Hamburger`), but you generally want a semantic version here. We treat this version as the _actual_ version of the app/library.

 SDK-style csproj files will automatically use the `Version` as the version of the nuget package it produces (if enabled to do so).

Here is the [Semantic Versioning](https://semver.org/) for changing major, minor, patch numbers.

How `Version`, `FileVersion` and `AssemblyVersion` are written:
`Version` : Major.Minor.Patch+suffix
`FileVersion`: Major.Minor.Patch.BuildVersion
`AssemblyVersion`: Major.0.0.0

##### This is How You Make Sense Of Alpha And Beta `FileVersion` suffix:
In file versioning, the use of alpha and beta suffixes typically indicates different stages of development or release. These suffixes are often used in software development to label versions that are in the testing or pre-release phase.

1. **Alpha Version:**
    
    - An alpha version is an early version of a software product that is still in the early stages of development.
    - It may contain incomplete features and might be unstable or buggy.
    - Alpha versions are usually released to a small group of internal testers or developers for initial testing and feedback.
2. **Beta Version:**
    
    - A beta version is a more advanced stage of development compared to alpha.
    - It generally signifies that the software has undergone initial testing and is considered feature-complete.
    - Beta versions are released to a wider audience, including external testers or a select group of users, to gather additional feedback and identify and fix bugs before the final release.

In summary, alpha versions are early and incomplete releases, often for internal testing, while beta versions are more polished and opened up to a broader audience for further testing and refinement before the official release. The use of alpha and beta labels helps developers and users understand the stage of development and the expected level of stability and completeness in a given version of the software.
#### **Assembly Culture:**

The AssemblyCulture attribute is used for specifying the culture of the assembly. By default in the .NET Framework assemblies are Language-Neutral which means the AssemblyCulture attribute contains Culture=neutral. If you go to the GAC, then you will find most of the assemblies are Culture-neutral. But there could be some assemblies that are culture-specific. 

When you specify the culture, then that assembly becomes a **Satellite Assembly**.

Using satellite assemblies, you can place resources for different languages in different assemblies, and the correct assembly is loaded into memory only if the user selects to view the application in that language

to change culture go to AssemblyInfo.cs and type : **[assembly: AssemblyCulture(“en”)]** (int this case the culture in english)

#### **Public Key Token:**

If you go to the GAC, then you will see that every assembly is assigned a public key token as shown in the below image.

###### **How do I get the Private-Public Key?**

use Strong Naming Tool (sn.exe) in the VS Command Prompt

Type **sn.exe -k D:\MyKeyFile.snk** and press enter

 the key file with the name `**MyKeyFile.snk**` should be generated in the D: Drive. In SN.exe, SN stands for Strong Name.
 
Once you generated the Key file, use the `**AssemblyKeyFile**` attribute in the `**AssemblyInfo**` class to sign the assembly with a strong name. 

To the constructor of the `AssemblyKeyFile` attribute, you need to pass the path of the key file that contains the private and public keys as shown below.

**[assembly: AssemblyKeyFile(“D:\\MyKeyFile.snk”)]**

Then  build the solution. Once you build the solution, now your assembly sign with a private-public key pair.



### Using Gacutil (Global Assembly Cache Utility) Tool to Install Assemblies

make sure you sign the assembly with a private public key. Then...

Build the solution.

**Gacutil -i assemblyFullPath.dll** will install the assembly. Use -u instead of -i to uninstall. (will probably be inside a bin\Debug folder)

If successful you will be able to find the assembly in this path: **C:\WINDOWS\Microsoft.NET\assembly\GAC_MSIL\AssemblyName\vWhateverTheFuckIt'sDifferentEveryTime

##### **How to use the SampleAssembly in other Projects?**

First, we need to add a reference to the SampleAssembly.dll from the location: **(C:\WINDOWS\Microsoft.NET\assembly\GAC_MSIL\SampleAssembly\v4.0_1.0.0.0__8f8fc05537931ee7**) where it is stored in GAC.

then just put:

```
using SampleAssembly;
```

in your program.

When uninstalling, If there are multiple versions of **SampleAssembly**in the GAC, then all those versions will be removed by the above command. If you want to remove only one of the assemblies then specify the full name as shown below.

**gacutil -u SampleAssembly,Version=1.0.0.0,PublicKeyToken=8f8fc05537931ee7**


# App Domain 

The App Domain (Application Domain) in .NET Framework is a logically isolated container inside which the .NET Code runs.

App Domain are super important because they allow you to run code that you don't trust in isolated containers (a specific App Domain for this untrustworthy code) that has it's own set of restrictions to stop this shady code from potentially damaging your machine.

When you run an application or .exe assembly the EXE runs as a Process inside the Operating System. Inside the Process, we have one App Domain by default loaded, and inside that App Domain the objects that you create in your program will be running (Obj1, Obj2 for example).

By default always there is an App Domain under which our .NET Code runs.

##### **How to Create a Custom App Domain in .NET Framework?**

Let's say we don't want a third-party code to have access to my D: Drive for safety reasons.

You would need to do this:

```csharp
using System;
namespace AppDomainDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Step1:
            //Create custom App Domain
            //CreateDomain: Creates a new application domain with the specified name.
            AppDomain customDomain = AppDomain.CreateDomain("customDomain");

            //Step2:
            //Get the Type of ThirdParty using the typeof method by passing the ThirdParty class name
            Type thirdParty = typeof(ThirdParty);

            //Step3:
            //Create an object of ThirdParty using the customDomain i.e. load the ThirdParty
            //To Create an Object, we need to call the CreateInstanceAndUnwrap method of customDomain object
            customDomain.CreateInstanceAndUnwrap(
                                  //Gets the display name of the assembly.
                                  thirdParty.Assembly.FullName,

                                  //Gets the fully qualified name of the type, including its namespace 
                                  //but not its assembly.
                                  thirdParty.FullName);

            //Step4:
            //Unload the Custom App Domain
            AppDomain.Unload(customDomain);

            //Own DLL
            MyClass1 obj1 = new MyClass1();
            MyClass2 obj2 = new MyClass2();

            Console.Read();
        }
    }

    [Serializable]
    public class ThirdParty
    {
        public ThirdParty()
        {
            Console.WriteLine("Third Party DLL Loaded");
            System.IO.File.Create(@"D:\xyz.txt");
        }

        ~ThirdParty()
        {
            Console.WriteLine("Third Party DLL Unloaded");
        }
    }

    public class MyClass1
    {
    }

    public class MyClass2
    {
    }
}
```

But it is not enough because the only thing you did is create a new App Domain you still need to write the logic to restrict the D: drive access for the code that will be run inside this App Domain

In order to restrict the Custom Application Domain to access D Drive, we need to create a permission object and restrict No Access to D Drive and then create a setup for the App Domain. And finally, we need to use both permissions and setup while creating the Custom Application Domain:

```csharp
using System;
using System.Security;
using System.Security.Permissions;
namespace AppDomainDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Step1: Create Permission object
            var permission = new PermissionSet(PermissionState.None);

            //Permission for the code to run. 
            //Execution: Without this permission, Managed Code will not be Executed.
            permission.AddPermission(
                new SecurityPermission(SecurityPermissionFlag.Execution)
                );

            //Set No Access to C Drive, 
            //NoAccess: No access to a file or directory.
            permission.AddPermission(
               new FileIOPermission(FileIOPermissionAccess.NoAccess, @"D:\")
               );

            //Step2: Create setup for App Domain
            var setUp = new AppDomainSetup
            {
                //CurrentDomain: Gets the current application domain
                //SetupInformation: Gets the application domain configuration information
                //ApplicationBase: Gets or sets the name of the directory containing the application.
                ApplicationBase = AppDomain.CurrentDomain.SetupInformation.ApplicationBase
            };

            //Step3: Create custom App Domain
            //Create custom App Domain using the setup and permission
            //CreateDomain: Creates a new application domain with the specified name.
            AppDomain customDomain = AppDomain.CreateDomain("customDomain", null, setUp, permission);

            //Step4:
            //Get the Type of ThirdParty using the typeof method by passing the ThirdParty class name
            Type thirdParty = typeof(ThirdParty);

            //Step5:
            //Create an object of ThirdParty using the customDomain i.e. load the ThirdParty
            //To Create an Object, we need to call the CreateInstanceAndUnwrap method of customDomain object
            customDomain.CreateInstanceAndUnwrap(
                                  //Gets the display name of the assembly.
                                  thirdParty.Assembly.FullName,

                                  //Gets the fully qualified name of the type, including its namespace 
                                  //but not its assembly.
                                  thirdParty.FullName);

            //Step6:
            //Unload the Custom App Domain
            AppDomain.Unload(customDomain);

            //Own DLL
            MyClass1 obj1 = new MyClass1();
            MyClass2 obj2 = new MyClass2();

            Console.Read();
        }
    }

    [Serializable]
    public class ThirdParty
    {
        public ThirdParty()
        {
            Console.WriteLine("Third Party DLL Loaded");
            System.IO.File.Create(@"D:\xyz.txt"); //this line will throw an exception now.
        }

        ~ThirdParty()
        {
            Console.WriteLine("Third Party DLL Unloaded");
        }
    }

    public class MyClass1
    {
    }

    public class MyClass2
    {
    }
}
```

if you want to execute the Default App Domain, then you need to put the logic of the Custom App Domain inside the try-catch block as shown in the below code:

```csharp
using System;
using System.Security;
using System.Security.Permissions;
namespace AppDomainDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Step1: Create Permission object
            var permission = new PermissionSet(PermissionState.None);

            //Permission for the code to run. 
            //Execution: Without this permission, Managed Code will not be Executed.
            permission.AddPermission(
                new SecurityPermission(SecurityPermissionFlag.Execution)
                );

            //Set No Access to C Drive, 
            //NoAccess: No access to a file or directory.
            permission.AddPermission(
               new FileIOPermission(FileIOPermissionAccess.NoAccess, @"D:\")
               );

            //Step2: Create setup for App Domain
            var setUp = new AppDomainSetup
            {
                //CurrentDomain: Gets the current application domain
                //SetupInformation: Gets the application domain configuration information
                //ApplicationBase: Gets or sets the name of the directory containing the application.
                ApplicationBase = AppDomain.CurrentDomain.SetupInformation.ApplicationBase
            };

            //Step3: Create custom App Domain
            //Create custom App Domain using the setup and permission
            //CreateDomain: Creates a new application domain with the specified name.
            AppDomain customDomain = AppDomain.CreateDomain("customDomain", null, setUp, permission);

            try
            {
                //Step4:
                //Get the Type of ThirdParty using the typeof method by passing the ThirdParty class name
                Type thirdParty = typeof(ThirdParty);

                //Step5:
                //Create an object of ThirdParty using the customDomain i.e. load the ThirdParty
                //To Create an Object, we need to call the CreateInstanceAndUnwrap method of customDomain object
                customDomain.CreateInstanceAndUnwrap(
                                      //Gets the display name of the assembly.
                                      thirdParty.Assembly.FullName,

                                      //Gets the fully qualified name of the type, including its namespace 
                                      //but not its assembly.
                                      thirdParty.FullName);
            }
            catch (Exception Ex)
            {
                Console.WriteLine($"Exception Occurred: {Ex.Message}");
                //Step6:
                //Unload the Custom App Domain
                AppDomain.Unload(customDomain);
            }

            //Own DLL
            MyClass1 obj1 = new MyClass1();
            MyClass2 obj2 = new MyClass2();

            Console.Read();
        }
    }

    [Serializable]
    public class ThirdParty
    {
        public ThirdParty()
        {
            Console.WriteLine("Third Party DLL Loaded");
            System.IO.File.Create(@"D:\xyz.txt");
        }

        ~ThirdParty()
        {
            Console.WriteLine("Third Party DLL Unloaded");
        }
    }

    public class MyClass1
    {
    }

    public class MyClass2
    {
    }
}
```

##### **Advantages of using App Domain in .NET Application:**

1. You can load and unload DLL inside these logical containers without one container affecting the other. So, if there are issues in one application domain you can unload that application domain, and the other application domain work without issues.
2. If you have a Third Party DLL and for some reason, you don’t trust the third-party code. You can run that DLL in an isolated app domain with fewer privileges. For example, you can say that the DLL cannot access your “D:\” drive. And other DLLs that you trust you can run with full privilege in a different app domain.
3. You can run different versions of DLL in every application domain.






# DLL Hell and Solution

## DLL Hell

DLL Hell  happens when 2 or more applications use the same .dll assembly and after a assembly update one or many or all applications break because of the changes in the .dll asssembly implementation are now generating erros in the other application.

## Solution

What generates the DLL Hell problem is the lack of a public private key pair. Because of the lack of a key, the different versions of the same assembly are not uniquely identifiable.

We need to sign the shared assemblies with a strong name. 

Therefore give a public private key token to the .dll assembly that is causing problems.

With Strongly Named Assembly, we can put different versions of the same assemblies into GAC (Global Assembly cache). So, if Application1 did any breaking change, the change will be in a different version of Shared Assembly, so it will not affect Application1.

Install the assembly in the GAC

Then in your program you are going to need to refer to the assembly in the GAC and not the .dll assembly that is in your solution (even do it could be the same assembly)

As we make some changes to the class library project, let us update the AssemblyVersion attribute of the AssemblyInfo class of the SharedAssembly class library project. Currently, the value of the AssemblyVersion attribute is “**1.0.0.0**“, let us update this value to “**2.0.0.0**” as follows.

Now, build the SharedAssembly class library project and again install the assembly in GAC

Now, if you go to the GAC, then you will see that two versions of the assembly are available

Now use one version for one application and another version for another application.


