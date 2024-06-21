Before focusing on this project I should finish "feeling ASP.NET + MVC + Blazor out" and getting comfortable with it. 

**Basic Idea behind the Project:** We know that in companies and big organizations and businesses, there are a lot of appointments on a daily basis. So, it becomes very difficult for the business person or employees of these organizations who are responsible for keeping a record of all the appointments and daily meetings. So, an Appointment Scheduler can be a web-based software application that can help these people in the effective management of their time and keeping records of all the appointments and meetings.

**Project Description:** This is a web-based project and allows the users to log in to the web page. After logging in, the users can add, edit or even delete the appointment schedules(DONE). The feature of SMS sending should be a must in this project to remind the user of an upcoming appointment with its date and time(). In order to make this more complete and advanced, authentication while logging in should be added to make it more secure(). Some of the key features of this project are

1. Users can Log In and Log Out of the application.(Implementable) 
    
2. Logging in requires a user id and password(On the way). The user ID should be a valid email ID and the password is of the user’s choice(On the way). There should be authentication and security while Logging In and an option to change passwords should also be provided to the users(I think it is on the way).  -> All of this needs Individual User Authentication that can be achieved with Identity in ASP.NET (other types of authentication. No Authentication, Windows Authentication (recommended for intranet applications), Microsoft Identity Platform(when corporate authentication is required))
    
3. Adding new appointments and deleting some of them (DONE). Auto-deletion of the appointment after the time has passed can also be an interesting feature (DONE). 
    
4. The list of all the upcoming appointments and the feature for searching for an appointment should be there.  (This is should be the content of the appointment page) - (DONE)
    
5. Sending SMS to the user prior to the appointment time.()
	- *I will not do this since its not safe to confirm authentication through SMS given that bad actors can easily fake SMS messages to the users*
	- *Time-based One Time and QR Code authentication are more desirable*. ASP.NET Core project templates that use Identity include multi-factor authentication support for TOTP authenticator apps. The Razor Pages template's **Configure authenticator app** form displays a 32-character registration key to seed the token value. However, the template doesn't generate a QR code by default.
1. Redirect the Index to the Details page when the earliest appointment is 10 minutes from now(I cannot make this work -> 
		This can be done with if UseSession() middleware is part of the request pipeline.
			This can be done with conditional map branching middleware that checks for the value of a boolean key in the query string of the HTTP request. Depending if the parameter is true or false maps the request to the Details page of the Appointment CRUD Pages. Check the [MapWhen Middleware](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/?view=aspnetcore-8.0) .
		
	.)

Steps to achieve Items 1 and 2:
- Add a new scaffolded Identity to the MVC application project.  
	- Choose the _Layout Page/View that you going to use in the resulting Pages of the scaffolding.
	- Create a new database or choose an existing database to store the Models used by Identity Razor Pages.
- Create a model for the User of the Application  (ApplicationUser class). And since it is data for the Identity Razor Pages stored it in the Data folder.
- Add new FirstName and LastName properties to the ApplicationUser model, add the necessary validation attributes to the properties, set one more attribute in the database connection string called TrustServerCertificate to true and Create a new migration and update the database
- Then you need to create a Layout Page that is reserved for the Identity Razor Pages (_AuthLayout.cshtml (obviously stored in the Pages folder))_ and you need to set the Layout property of RazorPageBase class to the Universal Shared Layout of the MVC Application.
	- Dont forget about @RenderBody and @section Scripts. Otherwise you will not get the desired output.
- Next you change the Layout property of the Identity Razor Pages to AuthLayout.cshtml
- Then you will user Bootstrap(collection of very helpfull html helpers that allow for css personalization directly within the html through built in classes) to change the default Identity Razor Pages UI to something better loooking.
- In the LogIn Identity Razor Page you need to use Bootstrap to create a good looking input for the FirstName and LastName User defined properties.
- Then you need to configure the Identity Service in Program.cs to not require Uppercase in it's passwords.
- change the font of the Universal Shared Layout View with google fonts. And apply the new font to the Identity Razor Pages through the Identity Specific Layout Page _AuthLayout.cshtml
- change the InputModel class in Register.cshtml.cs to have a FirstName and a LastName proprety. change Register.cshtml to allow the user to input their First Name and Last Name.
	- Dont forget to change the post method of the Razor Page to Bind the received inputas values to the FirstName and LastName properties of the created User.
- Remove the HTML code in Register.cshtml and LogIn.cshtml that became useless thanks to the _AuthLayout.cshtml layout.
- 
The link to a sample Appointment Scheduling Project along with its source code is given. You can take inspiration from it and try to create your own appointment scheduler using ASP.NET.