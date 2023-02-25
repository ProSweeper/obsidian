### Summary
- Full Stack application made with vanilla JavaScript on the front end and node + express for the backend with a mongoDB database
- Users can sign up a team for the tournament and add players to their roster, assigning the player a position and number
- The site is user centric and features google OAUTH 2.0 to allow a quick and easy sign in 
- only team creators can edit their team but anyone who is signed in can view the teams that are registered
### Inspiration
- My Girlfriend volunteers in the summer as an organizer for Alberta Indigenous games for the floor hockey tournament. Last summer was her first year and she was over whelmed by the task as there was a lack of organization and guidance from her superiors 
- I have a goal to create a streamlined website for teams to register and for the tourney info to be displayed and I wanted to make a prototype for my project
- The goal of the final version of this will be to ease her workload and potentially expand its functionality to encompass more tournaments in the future
- This was an open ended project for my general assembly schooling and the only requirements were using express and mongoDB, as well as having full CRUD functionality (create read update delete)
### Design Philosophy
- MVC design pattern to allow scalability and maintainability
- The user interacts with the  
### Things I would Change
- I would like to move to a full react app instead of vanilla JS and JSX for the front end
- Ideally I could collaborate with organizers to see what their problems they encounter often instead of making my best assumptions
### Features I would Like to Add
- I would like to add a bracket organizer and scheduler
- I would like to have the option to keep track of individual statistics
- I would like to implement a system to notify players (or parents) of upcoming games and their location
	- this would mean implementing a location feature, most likely using the google maps API
- I would like a more detailed player summary page
	- this could include past teams, tourneys ect...
- I would like the option to sign up for a free agent pool and then to add the option for teams to be assigned free agents if they need more players or for a free agent team to be created
- Have additional logic to better support multiple sports 
