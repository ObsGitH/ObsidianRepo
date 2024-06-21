1. Scaffold in the project the Identity Login and Register Razor Pages
2. Create a specific Layout page for the Identity Razor Pages and apply that layout to the Razor Pages
	1. JavaScript that applies to all Identity Razor Pages should be set here
	2. Do not forget the @section Scripts{}
3. Add new Properties to the ApplicationUser Model if needed
	1. Do not forget the necessary attributes
4. Set TrustServerCertificate attribute to true in the database connection string
5. Create migration and update the database
6. Add the User defined properties in the ApplicationUser model to the InputModel class of the Identity Razor Pages PageModels if you want the user to pass values to these user defined ApplicationUser properties - something that you probably want to do.
	1. Don't forget the necessary attributes
7. Modify the Razor Page View to allow the user to pass values to the User defined InputModel classes
	1. If the User defined propertie are not required in the UserAccount Model then they should not be required in the InputModel and they should not be targeted by jquery validation in the Razor Page View.
8. Modify the OnPostAsync() to pass the values of the InputModel user defined properties to the local Application User instance.
9. Disable Indentity account verification for an account that is being created for the first time.
10. Configure Identity Options in program.cs
	1. Change the configuration about  password, etc requirements in Identity in your application
11.  Go to `builder.Services.AddDefaultIdentity<ApplicationUser>(options => options.SignIn.RequireConfirmedAccount = true).AddEntityFrameworkStores<IdentityPracticeDBContext>();` 
	1. You can change and explore the other options/configurations as well. See what else you can find that is useful to you.
12. Add the _LoginPartial.cshtml created in the Identity scaffolding to the default Layout of the application in the place of the_
	1. Dont forget to delete Home and Privacy from _Layout.cshtml_ as well.
13. Put the Login and Register Razor Pages HTML inside a grid layout in order for them to look nicer
14. Go to the Browser Web Debugger -> Inspection -> Cookies -> scheme://host -> .AspNetCore.Identity.Application (the cookie that remembers the LoggedIn User) -> Expires -> Session == cookie lost when browser is closed  a date (2024/07/23) == date when the cookie is lost
	1.  If you click on remember me the cookie will expire after 14 days.
15. Through Dependency Injection, inject a `UserManager<ApplicationUser>` instance into the HomeController through it's constructor.
	1. `UserManager<ApplicationUser>` has a User property containing data of the Logged In User and also methods to use with the User property as well.
	2. This allows us to get the currently logged in user data/info to the Home Controller and therefore it's actions and the views that are connected to these actions as well.
	3. You will not need to this always, only when necessary.
	4. As an exercise pass the Logged In user Id to the HomeController Index View with the ViewData dictonary.
		1. The user Id is very important to track what the user is doing when they visit your Application.
16. Stop Unauthorized users from accessing the HomeController
	1. All you need is the Authorize attribute in the right place.
17. Now do the opposite: make authenticated users incapable of accessing the Login and Register Razor Pages. 
	1. Tip 1: what method loads the Identity Razor Pages?
	2. Tip 2: All PageModel classes have a User property that you will need to use.
	3. Tip 3: you are going to solve this problem with an if statement
	4. Tip 4: The PageModel property User is used for a lot of thing, so you need to find it's properties that relate to Identity ASP.NET API.
	5. Tip 5: The PageModel property Response allows you to get the response of the current Razor Page and redirect it.


## We only used the Login and Register Identity Scaffolded Views. If you want to use other Identity Views you can just: Add new scaffolded item to the project again -> Identity -> choose the Layout -> choose the Views -> choose the ClassModel -> Click on Add
## Forgot Password, 2-factor-authentication and Email Verification are not covered in this tutorial and need extra configuration and logic to work.
- To add the logic to these mechanisms consider exploring [Identity ASP.NET](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-7.0&tabs=visual-studio)
- To understanding authentication and the types of authenticaiton further, consider exploring [Authentication ASP.NET](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/?view=aspnetcore-7.0)

