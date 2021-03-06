OpenSupports can work completely fine without a frontend. You could only use this API to manage the system.
This documentation is intended for application developers who are looking to integrate OpenSupports with other applications.

## Request/Response Interface
The API server is located in the `api/` folder of your OpenSupports' instance folder.
**All the requests must be done via POST method**, with the exception of the path `/system/download`.

The response object has the following JSON structure:

```
{
  status: ["success"/"fail"],
  message: [String],
  data: [Object/Array/String]
}
```

* `status` indicates `"success"` if the request succeeded, else contains `"fail"`
* `message` contains the message in case of failure.
* `data` contains the returned data of the request.

For each path, we will document first the *Parameters*, then the *success* content of the property `data`, and finally the possible error messages it can return.

## Session Permissions
Some paths will not work if you're not logged in.
All paths have documented a *"Permission"* that can be:
* *any:* anyone can do a request, even if it's not logged in.
* *user:* only logged regular users can do the request.
* *staff1:* only a logged staff member with at least level 1 can do the request.
* *staff2:* only a logged staff member with at least level 2 can do the request.
* *staff3:* only a logged staff member with at least level 3 can do the request.

To login, user or staff, you have to make a request to [/user/login](#api-User-Login)
This request will return you the session data with an `userId` and a `token`. You will use these to validate the session every time you make a request that requires a logged user.

**All the paths that have permission `user`, `staff1`, `staff2` or `staff3`, require *userId* and *token* to be passed as parameter**. You need to pass them as `csrf_userid` and `csrf_token` respectively.

If you don't pass the userId and token, a `NO_PERMISSION` error will be returned.

Additionally, if there are no users (only staff members), you can check a ticket you created by providing your email and the ticketNumber to the `/ticket/check` path. This path will return you a `token` and `ticketNumber` you will use to comment, retrieve, or do any other operations to the ticket.
