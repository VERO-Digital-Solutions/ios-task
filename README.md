Hello dear iOS dev prospect!

This repository is supposed to act as a playground for your submission.
Before getting started, please make sure to use this repository as a template on which you will commit and push your code regularly. Once you are ready, please mail us back the link to your repository. 

Below, you will find the **Task** definition. Happy Hacking :computer: and Good Luck :shamrock:

# Task

Write a iOS application that connects to a remote API, downloads a certain set of resources, shows them in a list and provides some basic searching/filtering feature-set.
In particular, the app should:

- Request the resources located at `https://api.baubuddy.de/dev/index.php/v1/tasks/select` 
- Store them in an appropriate data structure that allows using the application offline
- Display all items in a list showing `task`, `title`, `description` and `colorCode` (which should be a view colored according to `colorCode`)
- The app should offer a search bar that allows searching for any of the class properties (even those, that are not visible to the user directly)
- The app should offer a menu item that allows scanning for QR-Codes
  - Upon successful scan, the search query should be set to the scanned text
- In order to refresh the data, the app should offer a pull-2-refresh functionality
  

### Authorization

It's mandatory for your requests towers the API to be authorized. You can find the required request below:

This is how it looks in `curl`:

```bash
curl --request POST \
  --url https://api.baubuddy.de/index.php/login \
  --header 'Authorization: Basic QVBJX0V4cGxvcmVyOjEyMzQ1NmlzQUxhbWVQYXNz' \
  --header 'Content-Type: application/json' \
  --data '{
        "username":"365",
        "password":"1"
}'
```

The response will contain a json object, having the access token in `json["oauth"]["access_token"]`. For all subsequent calls this has to be added to the request headers as `Authorization: Bearer {access_token}`.

A possible implementation in `Swift` could be the following. You don't have to copy over this one, feel free to indivualize it or use a different network library.

```swift
import Foundation

let headers = [
  "Authorization": "Basic QVBJX0V4cGxvcmVyOjEyMzQ1NmlzQUxhbWVQYXNz",
  "Content-Type": "application/json"
]
let parameters = [
  "username": "365",
  "password": "1"
] as [String : Any]

let postData = JSONSerialization.data(withJSONObject: parameters, options: [])

let request = NSMutableURLRequest(url: NSURL(string: "https://api.baubuddy.de/index.php/login")! as URL,
                                        cachePolicy: .useProtocolCachePolicy,
                                    timeoutInterval: 10.0)
request.httpMethod = "POST"
request.allHTTPHeaderFields = headers
request.httpBody = postData as Data

let session = URLSession.shared
let dataTask = session.dataTask(with: request as URLRequest, completionHandler: { (data, response, error) -> Void in
  if (error != nil) {
    print(error)
  } else {
    let httpResponse = response as? HTTPURLResponse
    print(httpResponse)
  }
})

dataTask.resume()
```
