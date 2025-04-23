This document describes which checks have been implemented for the API that processes posts, comments, and profile data.

#### 1. `GET /posts`
**Response code:** 200;<br>
**Response format:** JSON array of objects;<br>
**Structure check:** each object has `id`, `title`, `author`;<br>
**Data check:** `id` values are unique, numbers, not empty, `title` and `author` are strings, not empty (the assumption that a real API is being tested and the `id` coming as a string instead of a number is a problem);<br>
**Matching check:** `title` and `author` contain "Post" and "Author" (this check is included under the assumption that it is defined in the requirements);<br>
**Record count check:** matches expected number of posts (optional check, record count check is valid only when the dataset is in its original state);<br>
**Sort check:** posts are sorted by `id` (this check is included under the assumption that it is defined in the requirements).

#### 2.	`GET /posts/{id}`

**Response code:** 200 for existing `id`, 404 for not existing;<br>
**Structure check:** object has `id`, `title`, `author`;<br>
**Data check:** `id` value are number, not empty, `title` and `author` are strings, not empty;<br>
**Matching check:** `title` and `author` contain "Post" and "Author";<br>

#### 3. `POST /posts`

**Response code:** 201 for successful creation;<br>
**Structure check:** created object has `id`, `title`, `author`;<br>
**Data check:** `id` value are not empty, `title` and `author` are strings and have expected values;<br>
**Database check:** new post is added to the list, new post cannot be added without required fields and with fields that are too long (this check is included under the assumption that it is defined in the requirements, for example the estimated allowable field length is 30 symbols for the `title` and `author`).
	
#### 4.	`PUT /posts/{id}`
**Response code:** 200 for existing `id`, 404 for not existing;<br>
**Structure check:** updated object has `id`, `title`, `author`;<br>
**Data check:** `title` and `author` are strings and have expected values;<br>
**Database check:** post data is updated, new post cannot be updated with fields that are too long.
	
#### 5.	`DELETE /posts/{id}`
**Response code:** 200 or 204 for existing `id`, 404 for not existing;<br>
**Structure check:** response has `id`, `title`, `author`;<br>
**Database check:** post is removed; the post cannot be deleted again; the comments of the post were deleted along with the post (the assumption that cascading deletion should be implemented according to the requirements).

#### 6. `GET /comments`

**Response code:** 200;<br>
**Response format:** JSON array of objects;<br>
**Structure check:** each object has `id`, `body`, `postId`;<br>
**Data check:** `id` values are unique, numbers, not empty, `body` values are strings, not empty, `postId` values are numbers, not empty, have an existing post;<br>
**Sort check:** comments are sorted by `id`.
	
#### 7.	`GET /comments/{id}`
**Response code:** 200 for existing `id`, 404 for not existing;<br>
**Structure check:** object has `id`, `body`, `postId`;<br>
**Data check:** `id` value are number, not empty, `body` values are strings, not empty;<br>
	
#### 8.	`POST /comments`
**Response code:** 201 for successful creation;<br>
**Structure check:** created object has `id`, `body`, `postId`;<br>
**Data check:** `id` value are number, not empty, `body` and `postId` have expected values;<br>
**Database check:** comment is added for existing `postId`, not added for not existing and invalid `postId`, without required fields and with fields that are too long (for example the estimated allowable field length is 100 symbols for the `body` and 10 for the `postId`).

#### 9. `PUT /comments/{id}`

**Response code:** 200 for existing `id`, 404 for not existing;<br>
**Structure check:** updated object has `id`, `body`, `postId`;<br>
**Data check:** `body` and `postId` are strings and have expected values;<br>
**Database check:** comment is updated; comment is not updated if fields are too long.

#### 10. `DELETE /comments/{id}`

**Response code:** 200 or 204 for existing `id`, 404 for not exsisting;<br>
**Database check:** comment is removed; the comment cannot be removed again.

#### 11. `GET /profile`
**Response code:** 200;<br>
**Structure check:** object has `name`;<br>
**Data check:** `name` is not empty string.

Additional Tests:

#### 12. `PUT /profile`
**Response code:** 200;<br>
**Check:** request body contains `name`;<br>
**Response check:** returned updated object;<br>
**Database check:** `name` field is updated; `name` field cannot be updated if field are too long (for example the estimated allowable field length is 30 symbols for the `name`).