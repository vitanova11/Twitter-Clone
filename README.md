# Twitter-Clone
Part One: Fix Current Features
Step One: Understand the Model
Read models.py. Make a diagram of the four tables.

Note that the follows table has an unusual arrangement: it has two foreign keys to the same table. Why?

Using pdb, set a debugger in one of your routes, and hit it to pause code execution. From here, try out the various relationships set on the model classes. Get a feel for how they work.

Step Two: Fix Logout
Right now, there are links in the site to logout, but the logout route is not implemented.

On logout, it should flash a success message and redirect to the login page.

Step Three: Fix User Profile
The profile page for users works, but is missing a few things:

the location
the bio
the header image (which should be a background at the top)
Add these.

Step Four: Fix User Cards
On the followers, following, and list-users pages, the user cards need to show the bio for the users. Add this.

Step Five: Profile Edit
There are buttons throughout the site for editing your profile, but this is unimplemented.

It should ensure a user is logged on (you can see how this is done in other routes)
It should show a form with the following:
username
email
image_url
header_image_url
bio
password [see below]
It should check that that password is the valid password for the user—if not, it should flash an error and return to the homepage.
It should edit the user for all of these fields except password (ie, this is not an area where users can change their passwords–the password is only for checking if it is the current correct password.
On success, it should redirect to the user detail page.

Step Six: Fix Homepage
The homepage for logged-in-users should show the last 100 warbles only from the users that the logged-in user is following, and that user, rather than warbles from all users.

Step Seven: Research and Understand Login Strategy
Look over the code in app.py related to authentication.

How is the logged in user being kept track of?
What is Flask’s g object?
What is the purpose of add_user_to_g?
What does @app.before_request mean?
Part Two: Add Likes
Do This Without AJAX/JavaScript

Eventually, this would be a fine feature to integrate with JS/AJAX — that’s even a further study possibility!

For now, though: please build this as a pure backend feature in Flask. Liking a warble should NOT be done via AJAX right now.

Add a new feature that allows a user to “like” a warble. They should only be able to like warbles written by other users. They should put a star (or some other similar symbol) next to liked warbles.

They should be able to unlike a warble, by clicking on that star.

On a profile page, it should show how many warblers that user has liked, and this should link to a page showing their liked warbles.

Part Three: Add Tests
Add tests. You’ll need to proceed carefully here, since testing things like logging in and logging out will need to be tested using the session object.

Let’s briefly discuss a couple of things related to tests: how you should test, and what you should test. We created some (mostly empty) test files:

test_user_model.py
test_user_views.py
test_message_model.py
test_message_views.py
In this case, there are four test files: two for testing the models, and two for testing the routes/view-functions.

We’ve put some boilerplate code into two of these to help you get started.

To run a file containing unittests, you can run the command FLASK_ENV=production python -m unittest <name-of-python-file>.

(we set FLASK_ENV for this command, so it doesn’t use debug mode, and therefore won’t use the Debug Toolbar during our tests).

If you are having an error running tests (comment out the line in your app.py that uses the Debug Toolbar)

So that’s how you should test. But what, exactly, should you be testing? Let’s take the above file structure as an example.

For model tests, you can simply verify that models have the attributes you expect, and write tests for any model methods.

Here are some questions your tests should answer for the User model:

Does the repr method work as expected?
Does is_following successfully detect when user1 is following user2?
Does is_following successfully detect when user1 is not following user2?
Does is_followed_by successfully detect when user1 is followed by user2?
Does is_followed_by successfully detect when user1 is not followed by user2?
Does User.create successfully create a new user given valid credentials?
Does User.create fail to create a new user if any of the validations (e.g. uniqueness, non-nullable fields) fail?
Does User.authenticate successfully return a user when given a valid username and password?
Does User.authenticate fail to return a user when the username is invalid?
Does User.authenticate fail to return a user when the password is invalid?
Try to formulate a similar set of questions for the Message model.

For the routing and view function tests, things get a bit more complicated. You should make sure that requests to all the endpoints supported in the views files return valid responses. Start by testing that the response code is what you expect, then do some light HTML testing to make sure the response is what you expect.

You should also be testing authentication and authorization. Here are some examples of questions your view function tests should answer regarding these ideas:

When you’re logged in, can you see the follower / following pages for any user?
When you’re logged out, are you disallowed from visiting a user’s follower / following pages?
When you’re logged in, can you add a message as yourself?
When you’re logged in, can you delete a message as yourself?
When you’re logged out, are you prohibited from adding messages?
When you’re logged out, are you prohibited from deleting messages?
When you’re logged in, are you prohibiting from adding a message as another user?
When you’re logged in, are you prohibiting from deleting a message as another user?
(This isn’t necessarily an exhaustive list of the tests you should write, but it should be enough to get you started.)

These tests are a bit trickier to write because they require you to make requests in the test, and look through the response in order to verify that the HTML you get back from the server looks correct.
