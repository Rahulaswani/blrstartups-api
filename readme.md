#API Documentation

Trying to document api endpoints of blrstartups community

##Routes
---

Following are the availble routes 

###Posts
---
```
GET /posts
```
By default gets all posts of type discussion
Filter parameter controls the parameters used to query for post

* filter[posts_per_page]=20 //this will show 20 posts per page
* filter[discussion_tag]=blob //posts which have "blob" as disussion tag
* filter[s]=keyword //display posts that match the search term "keyword"
* filter[order]=ASC|DSC //
* filter[orderby]=modified // order by last modified date. (ID, author, date, comment_count)

//Complex date queries
// Posts dated 12th june 2014
* filter[date_query][0][year]=2014&filter[date_query][0][month]=6&filter[date_query][0][date]=12

//Return posts between 9AM to 5PM on weekdays
* filter[date_query][0][hour]=9&filter[date_query][0][compare]='>='&filter[date_query][1][hour]=17&filter[date_query][1][compare]='<='&filter[date_query][2][dayofweek][0]=2&filter[date_query][2][dayofweek][0]=6&filter[date_query][2][compare]=BETWEEN

//Returns posts made over a year agon but modified in the past month
* filter[date_query][0][column]=post_date_gmt&filter[date_query][0][before]=1 year ago&filter[date_query][1][column]=post_modified_gmt&filter[date_query][1][after]=1 month ago


```
GET /posts/:post_id
```

Gets post by post id.
author and comments are sideloaded

```
POST /posts
```

Provide all the info in data 
* data[title] - optional
* data[description] - required
* data[date] - Date and time the post was, or should be, published in local time. Date should be an RFC3339    timestamp](http://tools.ietf.org/html/rfc3339). Example: 2014-01-01T12:20:52Z. Default is the local date and time. (string) optional
* data[discussion_tags] - optional 

**Requires access_token**

```
POST /posts/:post_id/like
```

Like discussion
Toggles like / unlike

**Requires access_token**

```
POST /posts/:post_id/follow
```

Follow discussion
Toggles follow / unfollow

**Requires access_token**

```
GET /posts/:post_id/tags
```

Get tags of a post
---

```
POST /posts/:post_id/tags
```

Add new tags
* data['discussion_tags'] = "tag1, tag2, tag3"

**Requires access_token**

###Comments

```
GET /comments
```

Get comments by IDS
* ?ids[]=1&ids[]=2

```
POST /comments
```

Create New Comment
* data['content'] = "comment text"

**Requires access_token**

###Authenitcation
```
POST /auth/token
```

Authenticate user
* Require: username, password
* Right now we are sending FB access token as username and password
* on successful authorization returns in the following format
* {"access_token":"token_string","expires_in":3600,"token_type":"Bearer","scope":null,"refresh_token":"refresh_token"}

###Users
```
GET /users 
```

Get users list 
* ids=1,2,3,4

```
GET /users/:user_id
```

Get single user

```
GET /users/me
```
Return user linked with access_token
**Requires access_token