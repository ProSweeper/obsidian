### Summary
- Full Stack MERN (MongoDB Express React Node) app that allows users to browse over 1300 exercises (with instructional gifs) and save them into a work out plan. 
- These exercises are all stored in a database since the API I wanted to use limited the calls I was allowed to make and this was my way to work around that. 
- The site is user centric and features JWT authorization as well as secure storage of user password information through bcrypt and "salting" the password. this means we hash the password so that if someone gets a hold of our data base the users info is not compromised

### Inspiration
- One of my largest goals is to create a fitness app that is free and has features that are locked behind paid apps or subscriptions. This is kind of my prototype to get a feel on how I could set it up in the future
- We had an open ended project for our final capstone project for general assembly, the only requirements were that it utilized the MERN stack and it had 3/4 CRUD (create, read, update, delete) features. Finesse Fitness has all 4 

### Design Philosophy 
- Mono repo react app with MVC (model view controller) design pattern. this makes it easily scalable and maintainable due to separation of concerns
- essentially the user is shown the view, which is translating the data from the model, and they can interact with that view. their interactions are then passed to the controller which updates the model and then in turn updates the view for the user
- Most of the app state is managed at the top page level in react so that any changes in the child components will bubble up to the top level and be able to be passed down to the necessary child/sibling components 
- this allows for up to date information and avoids conflicting data

### Things I would Change
- I definitely need more experience planning and simplifying each individual step in creating the application. 
	- In this iteration I planned the general scope of the project and failed to break down the larger ideas into smaller ones to aid in their implementation.
	- This lead to a lot of problem solving on the fly and my solutions, while they did work for the current iteration, may not be as scalable as a fully fledged app needs. And perhaps took longer to implement than if I had taken more time to plan. 
- I feel like an application such as this could benefit from having more knowledge of the technologies available instead of being forced into one specific Stack. although it is possible that the MERN stack is an optimal one for this app type
- I would also like to fix the load times for the exercise gifs as they take a few seconds to load in 

### Features I would like to add in the future
- Finish the implementation of my filter function
- Finish the ability to log sets, reps, and weight
- Add visualization of user process in their workouts
- Add suggestions for exercises for muscles a user is potentially neglecting
- Add stretching and cool down suggestions based on a workout
- Configure an algorithm to potentially generate workouts based on user goals and input
- allow sharing of workouts among friends
- allow social interactions 