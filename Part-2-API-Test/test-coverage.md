This document describes which checks have been implemented for the API that processes posts, comments, and profile data.

#### 1. `GET /posts`
**Response code:** 200;<br>
**Response format:** JSON is array of objects;<br>
**Structure check:** each object has `id`, `title`, `author`;<br>
**Data check:** `id` values are unique, numbers, not empty, `title` and `author` are strings, not empty;<br>
**Matching check:** `title` and `author` contain "Post" and "Author";<br>
**Record count check:** matches expected number of posts.

#### 2.	`GET /posts/{id}`

**Response code:** 200 for existing `id`, 404 for not existing;<br>
**Structure check:** object has `id`, `title`, `author`;<br>
**Data check:** `id` value are number, not empty, `title` and `author` are strings, not empty;<br>
**Matching check:** `title` and `author` contain "Post" and "Author".<br>

#### 3. `POST /posts`

**Response code:** 201 for successful creation;<br>
**Structure check:** created object has `id`, `title`, `author`;<br>
**Data check:** `id` value are not empty, `title` and `author` are strings and have expected values;<br>
**Database check:** new post is added to the list.
	
#### 4.	`PUT /posts/{id}`
**Response code:** 200 for existing `id`, 404 for not existing;<br>
**Structure check:** updated object has `id`, `title`, `author`;<br>
**Data check:** `title` and `author` are strings and have expected values;<br>
**Database check:** post data is updated.
	
#### 5.	`DELETE /posts/{id}`
**Response code:** 200 or 204 for existing `id`, 404 for not existing;<br>
**Database check:** post is removed.

#### 6. `GET /comments`

**Response code:** 200;<br>
**Response format:** JSON is array of objects;<br>
**Structure check:** each object has `id`, `body`, `postId`;<br>
**Data check:** `id` values are unique, numbers, not empty, `body` values are strings, not empty, `postId` values are numbers, not empty, have an existing post;<br>
	
#### 7.	`GET /comments/{id}`
**Response code:** 200 for existing `id`, 404 for not existing;<br>
**Structure check:** object has `id`, `body`, `postId`;<br>
**Data check:** `id` value are number, not empty, `body` values are strings, not empty;<br>
	
#### 8.	`POST /comments`
**Response code:** 201 for successful creation;<br>
**Structure check:** created object has `id`, `body`, `postId`;<br>
**Data check:** `id` value are number, not empty, `body` and `postId` have expected values;<br>
**Database check:** comment is added for existing `postId`, not added for not existing.

#### 9. `PUT /comments/{id}`

**Response code:** 200 for existing `id`, 404 for not existing;<br>
**Structure check:** updated object has `id`, `body`, `postId`;<br>
**Data check:** `body` and `postId` are strings and have expected values;<br>
**Database check:** comment is updated.

#### 10. `DELETE /comments/{id}`

**Response code:** 200 or 204 for existing `id`, 404 for not exsisting;<br>
**Database check:** comment is removed.

#### 11. `GET /profile`
**Response code:** 200;<br>
**Structure check:** object has `name`;<br>
**Data check:** `name` is not empty string.

Additional Tests:

#### 12. `PUT /profile`
**Response code:** 200;<br>
**Check:** request body contains `name`;<br>
**Response check:** returned updated object;<br>
**Database check:** `name` field is updated.