# Introduction

This documentation provides a comprehensive overview of the API testing suite for herokuapp.com , featuring a Postman collection and environment tailored for testing functionalities related to student management. The API enables tasks like retrieving Booking information, creating new Booking, Token,Update Booking,Delete Booking.

# Summary
I have completed an API test of Get all Bookings, Create a new Booking, Token,Update Booking,Delete Booking, and finally Get Booking details https://herokuapp.com/


Within this API testing framework, Booking information is examined, and diverse tests are conducted using various HTTP methods such as POST, GET, DELETE, and PUT.

Summary: A total of 5 Test Scripts and 12 assertions were done. All of them passed with 1 failed tests and 0 skipped tests. The number of iteration was 1.
!![Screenshot_1](https://github.com/user-attachments/assets/09d7e139-7ab9-4687-9a44-20dfab1887d9)


# Requirements
Postman
https://www.postman.com/

Node JS
https://nodejs.org/en/

# Details



### Create Booking

Body

```{ 
{
	"firstname" : "{{firstName}}",
	"lastname" : "{{lastName}}",
	"totalprice" : {{totalPrice}},
	"depositpaid" : {{depositPaid}},
	"bookingdates" : {
    	"checkin" : "{{checkin}}",
    	"checkout" : "{{checkout}}"
	},
	"additionalneeds" : "{{addiNeed}}"
}
```

Pre-Request Scripts
```
//First Nmae

var firstName = pm.variables.replaceIn("{{$randomFirstName}}")

pm.environment.set("firstName",firstName)
console.log(firstName)

//Last Name

var lastName = pm.variables.replaceIn("{{$randomLastName}}")
pm.environment.set("lastName",lastName)
console.log(lastName)

//price
//var totalPrice = pm.variables.replaceIn("{{$randomPrice}}")
var totalPrice = pm.variables.replaceIn("{{$randomInt}}")
pm.environment.set("totalPrice",totalPrice)
console.log(totalPrice)

//depositpaid
var depositPaid = pm.variables.replaceIn("{{$randomBoolean}}")
pm.environment.set("depositPaid",depositPaid)
console.log(depositPaid)



//Date check in

const moment = require("moment")
const today = moment()
//console.log(today.format("YYYY-MM-DD"))
//console.log(today.add(1,'M').format("YYYY-MM-DD")) //present dater/monther sathe 1 jog hobe
//console.log(today.subtract(1,'M').format("YYYY-MM-DD"))
var checkin = today.add(1,'d').add(3,'M').format("YYYY-MM-DD")
pm.environment.set("checkin",checkin)

var checkout = today.add(2,'d').format("YYYY-MM-DD")
pm.environment.set("checkout",checkout)

//additional Need
var addiNeed = pm.variables.replaceIn("{{$randomWord}}")
pm.environment.set("addiNeed",addiNeed)
console.log(addiNeed)




```
Booking id

Post-response Script

```
var jsonData = pm.response.json()
pm.environment.set("id",jsonData.bookingid)


````


### Get Booking

**Post-response Script**


````
var statusCode = pm.response.code
console.log(statusCode)

if(statusCode==200){
   
var json = pm.response.json()

pm.test("First Name Validation", function(){
    pm.expect(json.firstname).to.eql(pm.environment.get("firstName"))
})

pm.test("Last Name Validation", function(){
    pm.expect(json.lastname).to.eql(pm.environment.get("lastName"))
})

pm.test("Total price Check" ,function(){
    pm.expect(json.totalprice.toString()).to.eql(pm.environment.get("totalPrice"))
})

//pm.test("Total price Check" ,function(){
   // pm.expect(json.totalprice).to.eql(parseInt(pm.environment.get("totalPrice")))
//})

pm.test("Deposit Paid Validation", function(){
    pm.expect(json.depositpaid.toString()).to.eql(pm.environment.get("depositPaid"))
})

pm.test("Check in Validation", function(){
    pm.expect(json.bookingdates.checkin).to.eql(pm.environment.get("checkin"))
})

pm.test("Check out Validation", function(){
    pm.expect(json.checkout).to.eql(pm.environment.get("checkout"))
})

}else if(statusCode==404){
 pm.test("Not Found")
}
else if(statusCode==201){
 pm.test("Created")
}
else if(statusCode==202){
 pm.test("Accepted")
}
else if(statusCode==204){
 pm.test("No Content")
}
else if(statusCode==301){
 pm.test("Moved Permanently")
}
 else if(statusCode==400){
 pm.test("Bad Request")
}
 else if(statusCode==401){
 pm.test("Unauthorized")
}
else if(statusCode==403){
 pm.test("Forbidden")
}
else if(statusCode==405){
 pm.test("Method Not Allowed")
}
else if(statusCode==408){
 pm.test("Request Timeout")
}
else if(statusCode==429){
 pm.test("Too Many Requests")
}
else if(statusCode==500){
 pm.test("Internal Server Error")
}
else{
  pm.test("Something went rong...")
}

`````


### Token
 Post-response Script
```
var jsonData = pm.response.json()
pm.environment.set("access_token",jsonData.token)
```
 Body
```
{
	"username": "admin",
	"password": "password123"
}

```



## Update Booking
Pre-request Script
```
var updated_firstName = pm.variables.replaceIn("{{$randomFirstName}}")
pm.environment.set("updated_firstName",updated_firstName)

var updated_lastName = pm.variables.replaceIn("{{$randomLastName}}")
console.log(updated_lastName)
pm.environment.set("updated_lastName",updated_lastName)

//price

var update_totalPrice = pm.variables.replaceIn("{{$randomInt}}")
pm.environment.set("update_totalPrice",update_totalPrice)
console.log(update_totalPrice)

```
Body
```
{
	"firstname" : "{{updated_firstName}}",
	"lastname" : "{{updated_lastName}}",
	"totalprice" : {{update_totalPrice}},
	"depositpaid" : {{depositPaid}},
	"bookingdates" : {
    	"checkin" : "{{checkin}}",
    	"checkout" : "{{checkout}}"
	},
	"additionalneeds" : "{{addiNeed}}"
}


```


## Verify After Update Booking
Post-response Script
```
var json = pm.response.json();

pm.test("First Name Validation Update", function(){
    pm.expect(json.firstname).to.eql(pm.environment.get("updated_firstName"));
});

pm.test("Last Name Validation Update", function(){
    pm.expect(json.lastname).to.eql(pm.environment.get("updated_lastName"));
});


pm.test("Total price Check" ,function(){
    pm.expect(json.totalprice).to.eql(parseInt(pm.environment.get("update_totalPrice")))
})

pm.test("Deposit Paid Validation", function(){
    pm.expect(json.depositpaid.toString()).to.eql(pm.environment.get("depositPaid"))
})


pm.test("Check in Validation", function(){
    pm.expect(json.bookingdates.checkin).to.eql(pm.environment.get("checkin"))
})


pm.test("Check out Validation", function(){
    pm.expect(json.bookingdates.checkout).to.eql(pm.environment.get("checkout"))
})

```


```
# Report Configure

**Using Newman**
Newman is a command-line Collection Runner for Postman. It enables you to run and test a Postman Collection directly from the command line.

Install Command
```
npm install -g newman
```
Run Command
- newman run “Path/CollectionName.json” -e Path/EnvironmentName.json
- newman run “Collection Link” -e “Path”/EnvironmentName.json

## Create HTML Report
### Install Command
```
npm install -g newman-reporter-html
```
Or
```
npm install -g newman-reporter-htmlextra
```
### Report Run Command
```
newman run CollectionName.json -e EnvironmentName.json -r HTML
```
Or
```
newman run CollectionName.json -e EnvironmentName.json -r htmlextra
```
!![_C__Users_esrat_OneDrive_Desktop_API_newman_Testing-2024-11-19-08-53-02-922-0 html (1)](https://github.com/user-attachments/assets/157b0f0e-03d8-46f0-b353-a46a34bacd9e)
