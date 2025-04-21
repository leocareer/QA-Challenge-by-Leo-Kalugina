This document describes which checks have been implemented for the API that processes posts, comments, and profile data.

#### 1. `GET /posts`
**Response code:** 200;<br>
**Response format:** JSON, array of objects;<br>
**Structure check:** each object contains `id`, `title`, `author`;<br>
**Data check:** `id` values are unique, `title` and `author` are strings, not empty;<br>
**Record count check:** matches expected number of posts.

#### 2.	`GET /posts/{id}`

**Response code:** 200 for existing `id`, 404 for non-existent;<br>
**Structure check:** object with `id`, `title`, `author`;<br>
**Matching check:** returned `id` matches requested one;<br>
**Data check:** `title` and `author` are non-empty strings.

#### 3. `POST /posts`

**Response code:** 201 on successful creation;<br>
**Check:** request body contains `title`, `author`;<br>
**Response check:** returned created object with new `id`, `title`, `author`;<br>
**Database check:** new post is added to the list.
	
#### 4.	`PUT /posts/{id}`
**Response code:** 200 for existing `id`, 404 for non-existent;<br>
**Check:** request body contains `title`, `author`;<br>
**Response check:** returned updated object;<br>
**Database check:** post data is updated.
	
#### 5.	`DELETE /posts/{id}`
**Response code:** 200 (or 204) for existing `id`, 404 for non-existent;<br>
**Database check:** post is removed from the list.

#### 6. `GET /comments`

**Response code:** 200;<br>
**Response format:** JSON, array of objects;<br>
**Structure check:** each object contains `id`, `body`, `postId`;<br>
**Data check:** `id` values are unique, `body` is a string, `postId` refers to an existing post;<br>
**Filter by postId:** returns only comments for the specified `postId`.
	
#### 7.	`GET /comments/{id}`
**Response code:** 200 for existing `id`, 404 for non-existent;<br>
**Structure check:** object with `id`, `body`, `postId`;<br>
**Data check:** `postId` refers to an existing post.
	
#### 8.	`POST /comments`
**Response code:** 201 on successful creation;<br>
**Check:** request body contains `body`, `postId`;<br>
**Response check:** returned created object with new `id`;<br>
**Database check:** comment is added, `postId` is valid.

#### 9. `PUT /comments/{id}`

**Response code:** 200 for existing `id`, 404 for non-existent;<br>
**Check:** request body contains `body`, `postId`;<br>
**Response check:** returned updated object;<br>
**Database check:** comment data is updated.

#### 10. `DELETE /comments/{id}`

**Response code:** 200 (or 204) for existing `id`, 404 for non-existent;<br>
**Database check:** comment is removed.

#### 11. `GET /profile`
**Response code:** 200;<br>
**Structure check:** object with field `name`;<br>
**Data check:** `name` is a non-empty string.

Additional Tests:

#### 12. `PUT /profile`
**Response code:** 200;<br>
**Check:** request body contains `name`;<br>
**Response check:** returned updated object;<br>
**Database check:** `name` field is updated.