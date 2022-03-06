Original App Design Project - README Template
===

# UCSD Dining Hall Review

## Table of Contents
1. [Overview](#Overview)
1. [Product Spec](#Product-Spec)
1. [Wireframes](#Wireframes)
2. [Schema](#Schema)

## Overview
### Description
UCSD Dining Hall Review App provides a platform for students to express their opinion on on-campus dinning hall. Students can choose the dining hall that they want to get food at based on the reviews. 

### App Evaluation
- **Category:** Review and Social Networking
- **Mobile:** This app would be primarily developed for mobile, however, we might do a web version of the app so that students can access the app through their computer
- **Story:** Users can view reviews on different dining hall on campus, and they would have the choice to write their own review 
- **Market:** UCSD students 
- **Habit:** This app can be used often with students who like to dine in at school dining hall  
- **Scope:** This app can potentially be an app for all colleges. We can potentially change the dining hall names and have other college students to use the app. 

## Product Spec

### 1. User Stories (Required and Optional)

**Required Must-have Stories**

* Account page for each user
* User login to their account to add review 
* View other users' review on dining halls

**Optional Nice-to-have Stories**

* Have user to rate each dining hall they reviewed
* Have a rating for each dining hall after gathering data from all users 

### 2. Screen Archetypes

* Login
    * Before writing a review, user would be led to the login page
* Register
   * User signs up or logs into their account
* Dining Hall Names
   * Contain the name of all dining hall 
* Review page 
   * Each dining hall would have its own review page
* Adding review page 
   * When user chooses to add a review to a dining hall, they are prompt to adding review page to add a new review

### 3. Navigation

**Tab Navigation** (Tab to Screen)

* Dining Hall Review 
* My Account 

**Flow Navigation** (Screen to Screen)

* Dining Hall Names -> Dining Hall Review Page -> Add Reviews -> Force to login 
    * Users are prompted to the Dining Hall Names page
    * User would choose a dining hall to view reviews on this dining hall 
    * User would have the option to add their own review of the dining hall they select
* My Account -> login page
   * If user is not logged in, they can click on the login page to login to their account

## Wireframes
### [BONUS] Digital Wireframes & Mockups
![](https://i.imgur.com/nAaqPpd.png)
![](https://i.imgur.com/vUoVBcd.png)

### [BONUS] Interactive Prototype

## Schema 
### Models
#### Post
   | Property      | Type     | Description |
   | ------------- | -------- | ------------|
   | content      | String   | Review content |
   | author_username        | Pointer to User| review author |
   | author_college        | String| college of user (optional) |
   | likesCount    | Number   | number of likes for the post |
   | dislikesCount    | Number   | number of dislikes for the post |
   | postedAt     | DateTime | date when post is created (default field) |
### Networking
- [Add list of network requests by screen ]
- [Create basic snippets for each Parse network request]
- [OPTIONAL: List endpoints if using existing API such as Yelp]
#### List of network requests by screen
   - Review Feed Screen
      - (Read/GET) Query all posts with matching college.
         ```swift
         let query = PFQuery(className:"Post")
         query.whereKey("college", equalTo: choosenCollege)
         query.order(byDescending: "createdAt")
         query.findObjectsInBackground { (posts: [PFObject]?, error: Error?) in
            if let error = error { 
               print(error.localizedDescription)
            } else if let posts = posts {
               print("Successfully retrieved \(posts.count) posts.")
           // TODO: Do something with posts...
            }
         }
         ```
      - (Create/POST) Like a post
      ```swift
      let query = PFQuery(className:"Post")
      query.getObjectInBackground(withId: currentPostId) { (post: PFObject?, error: Error?) in
          if let error = error {
              print(error.localizedDescription)
          } else if let post = post {
              post["likes"] += 1 // following cases on redo like and disagree only differ in this line.
              post.saveInBackground()
          }
      }
      ```
      - (Delete) Undo existing like
      - (Create/POST) Disike a post
      - (Delete) Undo existing dislike.  
   For the above 3 requests, see "Like a post".
   - Create Post Screen
      - (Create/POST) Create a new post object
      ```swift
      let post = PFObject(className:"Post")
      post["text"] = textField.text
      post["user"] = currentUser.username
      post["user_college"] = currentUser.college     // will be set to '' if not entered during sign up
      post["date"] = Date().string(format: "yyyy-MM-dd")
      post["likes"] = 0
      post["dislikes"] = 0
      post.saveInBackground { (succeeded, error)  in
          if (succeeded) {
              // The object has been saved.
          } else {
              // There was a problem, check error.description
          }
      }
      ```
