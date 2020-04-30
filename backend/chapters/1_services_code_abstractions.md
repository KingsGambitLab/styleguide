## Chapter 1 - Services & Code Abstraction:

### IMPORTANT POINTS: (TL;DR)
- Service objects are great for encapsulating complex objects, but that doesn't mean all heavy-duty logic requires a service object
  A bad approach would be to simply move a 500 line model straight into a service object.
  A good service object always follows the single responsibility principle
- If your code handles routing, params or any other controller-y things, then your code belongs in the controller
- If you are trying to share your code in different controllers, then use a concern/model method abstraction
- If your code is like a model that doesn't need persistence, then use non-ActiveRecord classes/models
- If your does one specific business action like _*"Generate PDF"*_ or _*"Adding Participants to Zoom meeting"*_ or _*"Fetch overall user performance"*_ or _*"Updating client data in Live"*_ etc then ho for service


### "A specific business action that does one thing. That’s our goal with service objects."

---
Please read through the following Q&A carefullty to understand services & code abstractions,

### A. Should a method be added in model?

Add the method in a model if the functionality belongs to an object or class instance of this model

_*Acceptable Example*_

If `User` is a class then methods like  `user.is_mentee?`(instance method) and `User.is_experienced?`(class method) are acceptable

Explanation: `User.is_experienced?` can also be defined on instance level but it is more intuitive to have it on class level and make the decision based on the grad_year argument Passed to it. On the other hand if we have more user specific data that needs to be considered while determining if the user is experienced then this can be written as a instance method.

**NOTE:** In the above give example, we tend to write these conditions in controllers/views/anywhere necessary. It is better to abstract the funtionality so that if the conditions change in future then they only have to modified in one place. This also increases readability and decreases bugs


---
### B. Should a piece of code be abstracted (in model/service/helper)?

Any piece of code should always be abstracted using a suitable design pattern accordingly. If you consider the above example of `User.is_experienced?`, we can easily write these conditions in controller/view. But we chose to abstract it so as to remove redundancies and make your code DRY(Don't Repeat Yourself).

_*Yet another Example:*_

Consider we adding participants to a Zoom meeting. We might think that this functionality can be added to `User` model or a `BatchLesson`/`Batch` model with user's as arguments etc. But what happens when we have to add participants from another code which is not related to `User`/`BatchLesson`? Right, it is not intuitive! 

So, inorder to make this better, we make a Service for handling Zoom operations. Now, this `ZoomService` will abstract functionalities like Add Particpants, Creating/Invalidating a Zoom Meeting, etc. This service can be used by any model(`Batch`/`BatchLesson`/`MenteeMentorSession` etc)

---
### C. Should you write a separate service for a piece of code/method?

Ask yourself these questions, "Does this functionality ONLY belong to this model?", "Can some other entity also use this?", "Can I have alternate implementations for the same functionality?". If any of the answer is YES, then go ahead and abstract that functionality to a Service/Library etc.

_*Check the above question for example*_


---
### D. What should be added in controller?

A controller is more like an interface to of our API. It should not contain any method definitions (even private methods). Rather a controller should only call to the right model where the data update happens. As a bottomline, "_*No Business Logic*_" should be written in controller.

_*Bad Example:*_

`app/controllers/web/user_controller.rb`
```
def get_experienced_users
  exp_users = User.where(:orgyear => [0..(Date.today.year - 1)]).or(:experience => [0..1000])
  render json: {
    experienced_users: exp_users
  } and return
end
```

_*Good Example*_

`app/models/user.rb`
```
.
.
def self.get_experienced_users
  return User.where(:orgyear => [0..(Date.today.year - 1)]).or(:experience => [0..1000])
end
.
.
```

`app/controllers/web/user_controller.rb`
```
def get_experienced_users
  exp_users = User.get_experienced_users
  render json: {
    experienced_users: exp_users
  } and return
end
```

---
### E. When to define class methods and when to abstract a class method?

Again, it is the same explanation, if you are worried that there might be many class methods and the model will be bloated, then you can delegate some functionality to a service. Go through the example below

_*Initial Scenario:*_

`app/models/user.rb`
```
.
.
def self.create_user(profile_type, password)
  # User creation logic goes here and all the handling for duplicate users can also be added too. error/creation messages will also be added here
end
.
.
```

_*After using a Service:*_

`app/services/user/register_user.rb`
```
class UserService::RegisterUser
  def initialize(user, profile_type, password)
    @user, @profile_type, @password = user, profile_type, password
  end
  
  def execute
    # create a user and return a standard format response with success/error messages and any data that you want to
  end
end
```

`app/models/user.rb`
```
.
.
def create_new_user(profile_type, password)
  return UserService:RegisterUser.new(User.new(params)).execute
end
.
.
```
