# curl_examples
Say we have a REST API on localhost on port 8000. Below are some example network calls using curl for GET, PUT, POST, etc. If you are on Windows, it helps to run these in either Git Bash or Windows Subsystem for Linux. Otherwise, you may get some syntax issues

# HTTP GET
The default for curl is GET. You can retreive publically available information from an API as shown below. In this case, we would get some JSON representing the API's products
```
$ curl http://localhost:8000/api/products
```

# search an API
Many APIs will allow you to search for specific information. In this example, we are searching for a specific type of product, just by adding the search term to the URL
```
$ curl http://localhost:8000/api/products/coolproductsearchterm
```

# normal HTTP POST
If the API lets public users enter in information, you can use a POST request as shown below. We create a product on the API. The API will typically determine an id for that product on its own using UUIDs or auto incrementing integers. The --data allows you to put in whatever JSON information you need. The --header(s) that designate application/json help ensure the curl command is accepted by the API. They are not always needed
```
$ curl --header "Content-Type: application/json" \
--header "Accept: application/json" \
--request POST \
--data '{"name":"somename", "slug":"x", "description":"productdesc", "price":"99.77"}' \
http://localhost:8000/api/products
```

# normal HTTP PUT (updating data)
To update data that we just posted, we typically use HTTP PUT. We append the product ID to the URL and have JSON containing the new information
```
$ curl --header "Content-Type: application/json" \
--request PUT --data '{"name":"updated", "slug":"fdsx", "description":"desc", "price":"99.99"}' \
http://localhost:8000/api/products/1
```

# normal HTTP DELETE
The example below deletes the product with id = 2
```
$ curl --header "Content-Type: application/json" --request DELETE http://localhost:8000/api/products/2
```

# register
Below is an example for registering on our REST API. 
```
$ curl --header "Content-Type: application/json" \
--header "Accept: application/json" \
--request POST \
--data '{"name":"user1", "email":"cool@gmail.com", "password":"blahblah", "password_confirmation":"blahblah"}' \
http://localhost:8000/api/register
```

# login
This example sends a POST request to a login route for the API to obtain a Authorization Bearer token
```
$ curl --header "Content-Type: application/json" \
--header "Accept: application/json" \
--request POST \
--data '{"email":"cool@gmail.com", "password":"blahblah"}' \
http://localhost:8000/api/login
```

# protected POST
After you login, many APIs may issue you an auth token as a result from the request above. This token will be required to access protected parts of the API. Here is an example request using the token. You can only POST to products with a valid token
```
$ curl --header "Content-Type: application/json" \
--header "Accept: application/json" \
--header "Authorization: Bearer 2|OvugB49lsaBF24auL2NjAH51aQGsBp5mKCUVcstN" \
--request POST \
--data '{"name":"xwithauth", "slug":"x", "description":"descwithauth", "price":"99.99"}' \
http://localhost:8000/api/products
```

# logout
Below is an example of logging out of the API. This typically means invalidating, deleting, or reseting tokens
```
$ curl --header "Content-Type: application/json" \
--header "Accept: application/json" \
--header "Authorization: Bearer 2|OvugB49lsaBF24auL2NjAH51aQGsBp5mKCUVcstN" \
--request POST \
http://localhost:8000/api/logout
```