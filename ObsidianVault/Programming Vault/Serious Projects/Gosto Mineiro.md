This will be my first serious project!

This project is going to take a while to finish. Don't rush it. It is also a good idea to finish the projects that you already started before dabbling in this one. This one will be huge! And potentially profitable!

I advise you to finish: List appointments project, Revise the finished ConversationSimulator project to see if it is good enough for distribution then remake the BlackJack game or at the very least fix the console game and then finish BlackJackMVC project from start to finish.

They don't need to be perfect or fancy. They just need to look good and be logical.
Create these webpages in your ASP.NET Projects since ASP.NET web pages usually already use bootstrap by default.

It should be a website made by Mineiros to Mineiros. A website where only people from Minas Gerais can access. 

## How I will make businesses and people here in Minas Gerais be Interested in the Project

1. Businesses
	1.  I will make sure that businesses that give my site a chance will be able to enjoy the services that my website provides for free. This will be true for the websites that subscribe through the internet as well.
	2. The fees will only start to come when demand increases significantly: when companies start to contact me to put their restaurants in my Application.
	3. Then depending on how many companies agreed to subscribe to my idea when I was a nobody, they will also enjoy more lenient fees for 1 or 2 years to foster a good relantionship between me and the businesses that gave me a shot.\
	4. It is also important for businesses to be able to unsubscribe from my services whenever they wish to do so.
		1.  Maybe if the idea blows up, maybe I should create contracts with the companies and charge them if they want to unsubscribed earlier than what is allowed in the contract.
2. Consumers
	1. The exclusivity of the website should be very attractive for the mineiros
		1. I should make the users feel like they are special for being able to have an account in my website
		2. It should be an appeal to status/"patriotism"/exclusivity
		3. I should make the point that the ratings and the comments will more accurately represent the judgement and taste of mineiros since the ratings and the comments will be done by mineiros only.
	2. Later I can use the status of the restaurants that subscribed to my business and the quality of them AND the quality of the website itself to convince even more mineiros from subscribing to my website/app.
	3. At first the subscription should be free.
		1. When the demand increases enough to justify charging fees to the user's, I should also give benefit's for the users that were already using the website before the fees started to be charged in order to acquire their willingness to be charged with the fees in the future.
			1. The benefit should be of 1 or 2 years.
		2. Users should have all of their information deleted whenever they want to quit the website. In others words the information should be stored in a server side database that I have complete control of.
3. Fees
	1. Business Fees
		1. For the beginning of the business, the fees should be a percentage of the profit that the companies make from my website.
		2. Later when demand increases either the fees will increase or the fees will be charged on the Dinheiro Bruto gasto pelos usuarios nesse restaurante pelos meus usuarios

## How to create the Application

1. Restaurantes de Minas Gerais
	- Create a website with ASP.NET MVC. 
	- UI
		- I want the website to be very colorful in a brazilian way as well.
		- You look for AI that generate icons and brand icons and images for your website.
		- Layout
			- Nav-bar - HAS TO LOOK REALLY GOOD
				- Since the first page the users will land on is the hero page. You should make the Real Home Page a really special and flashy nav-item/nav-link
				- My brand icon
				- Title of the website
				- Links to my github, linkedin and other contacts
				- Make the nav-bar and everything in it look pretty
			- Footer
				- License
				- Privacy
			
		- Hero Page
			- Create a hero-page: a very nice looking Home Page with the sole purpose of creating a good impression and impressing the user. 
				- This incentivizes the user to use your website and consume it's services. It also incentivizes the use to come back
			- The use of ai generated images would be huge! You don't need to pay them anything since it is ai generated they cannot tell if they created them.
			- The hero page vibe needs to be mineira
			- The hero page should be related to food MG or both.
		- Real Home Page
			- Grid Layout
			- Filter related elements in the topmost row of the grid
				- one control to filter restaurant cards by name
				- one control to filter restaurant cards by city
				- one control to filter restaurant cards by rating
				- Maybe more filter controls are needed but I cannot think of any other filter right now.
			- Cards for every row of the grid
				- card
					- card-header 
					- card-body 
						- Picture/Pictures of restaurant right after the name of the restaurant
						- card-text
							-  Name of the Restaurant
							- Address
						- Rating -> it has to be some sort of responsive button/control and be displayed as those five stars icon as well
						- card-link
							- Phone Number (obviously in the form of an icon)
						- - maybe the card-body should be a Grid layout:
							- Picture at top-most row
							- Name of the restaurant in upper middle row
							- Rating + Phone Number in the bottom middle row
							- Address at the bottom row
					- It will contain filter/search logic so the cards that appear in the page will need to be updated/changed/re-generated every time a research is done and every time you access the webpage. (Translation: the cards will need to be updated for every HTTP POST and GET Request)
						- You already learned how to do this in the microsoft tutorials. If you have any trouble implementing this logic you should read the Microsoft MVC docs again.
						- You need to understand how Bootstrap cards work better.
				- There should be pagination at the bottom (Pagination can be implemented with Bootstrap)
		- Restaurant Pages
			- 
		- Log in/out Page and Register Pages or Page
			- They should be done with identity.
			- They should have a special layout that uses the global layout as reference 
		- Chat Room - A chat room where mineiros can discuss about the quality of the food and services of the restaurant
			
	- Logic
		
		- Models
			-  A restaurant class model where all of the necessary information of the restaurant will be held. 
				- It needs to be very serious, you cannot miss any type of important data about the restaurant, including Pessoa Juridica data to verify the legitimacy of the business.
			- A user model that will hold all the data that is necessary to identify, authenticate, authorize and garantee that the use is from Minas Gerais.
				- Same thing with the restaurant model, you will definetely need to acquire Personal Data for the model. Which you will need to ask the people accessing your website for permission.
		- Controller / Action
		- Database
			- A database will definitely be needed to store the information of the user and the restaurant.
		- Views
			- Hero Page
			- Real Home Page
		
