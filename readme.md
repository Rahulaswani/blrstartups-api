#API Documentation

Trying to document api endpoints of blrstartups community. When i coded this api we were doing alot of experiments, so you will find this not as per the REST api guidelines.

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

Normal Post
```json
{
    id: 40862,
    title: "",
    type: "discussion",
    author: 516,
    description: "Will you crowdfund this ideas at prototype stage? Give your views &amp; what validations will you look for before putting your credit card",
    date: "2015-04-30T04:35:49+00:00",
    modified: "2015-05-01T04:52:38+00:00",
    format: "standard",
    slug: "40862",
    excerpt: "Will you crowdfund this ideas at prototype stage? Give your views &amp; what validations will you look for before putting your credit card 26 Innovative Ideas By School Students That Will Blow Your Mind Away! These young geniuses could't wait to grow old to innovate something, so they started early. See these amazing 26 innovations [&hellip;]",
    comment_count: "4",
    like_count: null,
    comments: [
    "178753",
    "178769",
    "178788",
    "178814"
    ],
    post_belongs_to: [ ],
    discussion_tags: [ ], 
    meta_link: "{"picture": "https://fbexternal-a.akamaihd.net/safe_image.php?d=AQBxCPU48Fq2bjWW&w=130&h=130&url=http%3A%2F%2Fwww.thebetterindia.com%2Fwp-content%2Fuploads%2F2014%2F06%2FBULB-REMOVER.png&cfs=1&sx=573&sy=0&sw=627&sh=627", "description": "These young geniuses could't wait to grow old to innovate something, so they started early. See these amazing 26 innovations by school students across India.", "caption": "www.thebetterindia.com", "source": null, "link": "http://www.thebetterindia.com/11596/ignite-innovations/", "name": "26 Innovative Ideas By School Students That Will Blow Your Mind Away!"}",
    meta_post_is: "link",
    meta_fb_post_id: "220266924662120_934391466582992", //This key let us know that this post is present in fb too.
    date_tz: "UTC",
    date_gmt: "2015-04-30T04:35:49+00:00",
    modified_tz: "UTC",
    modified_gmt: "2015-05-01T04:52:38+00:00"
}
```

Job Post - By default every post is discussion post but if key "meta_post_format" present and value is job then my friend it is a job post.

```json
{
    id: 40872,
    title: "Android developer required (2+ years experience)",
    type: "discussion",
    author: 52075,
    description: "Looking for an android developer for creating a mobile app from scratch, for our startup Your D.O.S.T (www.yourdost.com). Your D.O.S.T is more than a business for us and is very close to our heart. We are IIT, IIM alumus with 5+ years of experience, passionate about the idea and have left our jobs to start this venture. We are looking for hardworking individuals who believe in the idea and will be equally passionate to work in a startup culture. We are looking for people with past experience in building android apps.",
    date: "2015-04-30T08:28:08+00:00",
    modified: "2015-04-30T08:28:08+00:00",
    format: "standard",
    slug: "android-developer-required-2-years-experience",
    excerpt: "Looking for an android developer for creating a mobile app from scratch, for our startup Your D.O.S.T (www.yourdost.com). Your D.O.S.T is more than a business for us and is very close to our heart. We are IIT, IIM alumus with 5+ years of experience, passionate about the idea and have left our jobs to start [&hellip;]",
    comment_count: "0",
    like_count: null,
    comments: [ ],
    post_belongs_to: [ ],
    discussion_tags: [
    "4"
    ],
    meta_job_location: "Bangalore",
    meta_job_candidate_submit_what: "Please submit a link to your resume and a short note on the relevant past experience (links to the mobile apps created in the past)",
    meta_company_name: "Your D.O.S.T",
    meta_company_url: "www.yourdost.com",
    meta_job_pay: "job_pay_recurring",
    meta_job_pay_currency: "inr",
    meta_job_pay_min: "₹ 240,000",
    meta_job_pay_max: "₹ 480,000",
    meta_job_equity_min: "0.0%",
    meta_job_equity_max: "100.0%",
    meta_post_format: "job",
    date_tz: "UTC",
    date_gmt: "2015-04-30T08:28:08+00:00",
    modified_tz: "UTC",
    modified_gmt: "2015-04-30T08:28:08+00:00"
}
```

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