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

### Get Booking

**Tests Script**


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


// pm.test("Deposit Paid Validation" ,function(){
//     pm.expect(pm.environment.get("depositPaid")).to.equal(json.depositpaid.toString())
// })

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

````
Create Booking

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
Tests Script
```
var jsonData = pm.response.json()
pm.environment.set("id",jsonData.bookingid)

```
Token
```
var jsonData = pm.response.json()
pm.environment.set("access_token",jsonData.token)


## Get Specific Student
Tests Script
```
var status_code = pm.response.code
if(status_code==200){
pm.test("200 ; STATUS : SUCCESS", function () {
    pm.response.to.have.status(200);
});
var jsonData = pm.response.json()
pm.test("ID Validation", function () {
    pm.expect(jsonData.data.id).to.eql(Number(pm.environment.get("id")));
});
pm.test("First Name Validation", function () {
    pm.expect(jsonData.data.first_name).to.eql(pm.environment.get("first_name"));
});
pm.test("Middle Name Validation", function () {
    pm.expect(jsonData.data.middle_name).to.eql(pm.environment.get("middle_name"));
});
pm.test("Last Name Validation", function () {
    pm.expect(jsonData.data.last_name).to.eql(pm.environment.get("last_name"));
});
pm.test("Date of Birth Validation" ,function () {
    pm.expect(jsonData.data.date_of_birth).to.eql(pm.environment.get("date_of_birth"));
});
}
else if(status_code==201){
    pm.test("201 ; STATUS : CREATED", function () {
    pm.response.to.have.status(201);
})}
else if(status_code==202){
    pm.test("202 ; STATUS : ACCEPTED", function () {
    pm.response.to.have.status(202);
})}
else if(status_code==400){
    pm.test("400 ; STATUS : BAD REQUEST", function () {
    pm.response.to.have.status(400);
})}
else if(status_code==401){
    pm.test("401 ; STATUS : UNAUTHORIZED", function () {
    pm.response.to.have.status(401);
})}
else if(status_code==402){
    pm.test("402 ; STATUS : PAYMENT REQUIRED", function () {
    pm.response.to.have.status(402);
})}
else if(status_code==403){
    pm.test("403 ; STATUS : FORBIDDEN", function () {
    pm.response.to.have.status(403);
})}
else if(status_code==404){
    pm.test("404 ; STATUS : NOT FOUND", function () {
    pm.response.to.have.status(404);
})}
else if(status_code==405){
    pm.test("405 ; STATUS : METHOD NOT ALLOWED", function () {
    pm.response.to.have.status(405);
})}
else if(status_code==500){
    pm.test("500 ; STATUS : INTERNAL SERVER ERROR", function () {
    pm.response.to.have.status(500);
})}
else if(status_code==501){
    pm.test("501 ; STATUS : NOT IMPLEMENTED", function () {
    pm.response.to.have.status(501);
})}
else if(status_code==502){
    pm.test("502 ; STATUS : BAD GATEWAY", function () {
    pm.response.to.have.status(502);
})}
else if(status_code==503){
    pm.test("503 ; STATUS : SERVICE UNAVAILABLE", function () {
    pm.response.to.have.status(503);
})}
```
## Create Technical Skills
Body
```
{ 
"id": 1, 
"language": [ 
"{{language_code_1}}", 
"{{language_code_2}}" 
], 
"yearexp": "{{yearexp}}", 
"lastused": "{{lastused}}", 
"st_id": {{id}}
}
```
Pre-Request Scripts
```
var language_1 = pm.variables.replaceIn("{{$randomLocale}}")
console.log("Language 1: "+language_1)
pm.environment.set("language_code_1",language_1)

var language_2 = pm.variables.replaceIn("{{$randomLocale}}")
console.log("Language 2: "+language_2)
pm.environment.set("language_code_2",language_2)

var year_exp = pm.variables.replaceIn("{{$randomInt}}")
console.log("Year of Experience: "+year_exp)
pm.environment.set("yearexp",year_exp)

const moment = require('moment')
const today = moment()
console.log(today.format("YYYY-MM-DD"))
pm.environment.set("lastused", today.format("YYYY-MM-DD"))
```
Tests Script
```
var status_code = pm.response.code
if(status_code==200){
pm.test("200 ; STATUS : SUCCESS", function () {
    pm.response.to.have.status(200);
});
}
else if(status_code==201){
    pm.test("201 ; STATUS : CREATED", function () {
    pm.response.to.have.status(201);
})}
else if(status_code==202){
    pm.test("202 ; STATUS : ACCEPTED", function () {
    pm.response.to.have.status(202);
})}
else if(status_code==400){
    pm.test("400 ; STATUS : BAD REQUEST", function () {
    pm.response.to.have.status(400);
})}
else if(status_code==401){
    pm.test("401 ; STATUS : UNAUTHORIZED", function () {
    pm.response.to.have.status(401);
})}
else if(status_code==402){
    pm.test("402 ; STATUS : PAYMENT REQUIRED", function () {
    pm.response.to.have.status(402);
})}
else if(status_code==403){
    pm.test("403 ; STATUS : FORBIDDEN", function () {
    pm.response.to.have.status(403);
})}
else if(status_code==404){
    pm.test("404 ; STATUS : NOT FOUND", function () {
    pm.response.to.have.status(404);
})}
else if(status_code==405){
    pm.test("405 ; STATUS : METHOD NOT ALLOWED", function () {
    pm.response.to.have.status(405);
})}
else if(status_code==500){
    pm.test("500 ; STATUS : INTERNAL SERVER ERROR", function () {
    pm.response.to.have.status(500);
})}
else if(status_code==501){
    pm.test("501 ; STATUS : NOT IMPLEMENTED", function () {
    pm.response.to.have.status(501);
})}
else if(status_code==502){
    pm.test("502 ; STATUS : BAD GATEWAY", function () {
    pm.response.to.have.status(502);
})}
else if(status_code==503){
    pm.test("503 ; STATUS : SERVICE UNAVAILABLE", function () {
    pm.response.to.have.status(503);
})}
```
## Create Student Address
Body
```
{ 
"Permanent_Address": { 
"House_Number": "{{House_Number}}",
"City": "{{City}}",
 "State": "{{State}}", 
"Country": "{{Country}}",
"PhoneNumber": [ 
{ 
"Std_Code": "{{Std_Code}}",
"Home": "{{Home}}",
 "Mobile": "{{Mobile}}" 
},
{ 
"Std_Code": "{{Std_Code_1}}",
"Home": "{{Home_1}}", 
"Mobile": "{{Mobile_1}}" 
} 
] 
},
"stId": {{id}}
}
```
Pre-Request Scripts
```
var house_number = pm.variables.replaceIn("{{$randomInt}}")
console.log("House Number: "+house_number)
pm.environment.set("House_Number",house_number)

var city = pm.variables.replaceIn("{{$randomCity}}")
console.log("CITY: "+city)
pm.environment.set("City",city)

var state = pm.variables.replaceIn("{{$randomStreetName}}")
console.log("STATE: "+state)
pm.environment.set("State",state)

var country = pm.variables.replaceIn("{{$randomCountry}}")
console.log("COUNTRY: "+country)
pm.environment.set("Country",country)

var std_code = pm.variables.replaceIn("{{$randomCountryCode}}")
console.log("STD CODE 1: "+std_code )
pm.environment.set("Std_Code",std_code )

var home = pm.variables.replaceIn("{{$randomStreetAddress}}")
console.log("HOME 1: "+home )
pm.environment.set("Home",home)

var mobile = pm.variables.replaceIn("{{$randomPhoneNumberExt}}")
console.log("MOBILE No. 1: "+mobile )
pm.environment.set("Mobile",mobile)

var std_code1 = pm.variables.replaceIn("{{$randomCountryCode}}")
console.log("STD CODE 2: "+std_code1 )
pm.environment.set("Std_Code_1",std_code1 )

var home1 = pm.variables.replaceIn("{{$randomStreetAddress}}")
console.log("HOME 2: "+home1 )
pm.environment.set("Home_1",home1)

var mobile1 = pm.variables.replaceIn("{{$randomPhoneNumberExt}}")
console.log("MOBILE No. 2: "+mobile1 )
pm.environment.set("Mobile_1",mobile1)
```
Tests Script
```
var status_code = pm.response.code
if(status_code==200){
pm.test("200 ; STATUS : SUCCESS", function () {
    pm.response.to.have.status(200);
});
var jsonData = pm.response.json()
pm.test("Status Validation", function () {
    pm.expect(jsonData.status).to.eql("true");
});
pm.test("MSG Validation", function () {
    pm.expect(jsonData.msg).to.eql("Add  data success");
});
pm.t
}
else if(status_code==201){
    pm.test("201 ; STATUS : CREATED", function () {
    pm.response.to.have.status(201);
})}
else if(status_code==202){
    pm.test("202 ; STATUS : ACCEPTED", function () {
    pm.response.to.have.status(202);
})}
else if(status_code==400){
    pm.test("400 ; STATUS : BAD REQUEST", function () {
    pm.response.to.have.status(400);
})}
else if(status_code==401){
    pm.test("401 ; STATUS : UNAUTHORIZED", function () {
    pm.response.to.have.status(401);
})}
else if(status_code==402){
    pm.test("402 ; STATUS : PAYMENT REQUIRED", function () {
    pm.response.to.have.status(402);
})}
else if(status_code==403){
    pm.test("403 ; STATUS : FORBIDDEN", function () {
    pm.response.to.have.status(403);
})}
else if(status_code==404){
    pm.test("404 ; STATUS : NOT FOUND", function () {
    pm.response.to.have.status(404);
})}
else if(status_code==405){
    pm.test("405 ; STATUS : METHOD NOT ALLOWED", function () {
    pm.response.to.have.status(405);
})}
else if(status_code==500){
    pm.test("500 ; STATUS : INTERNAL SERVER ERROR", function () {
    pm.response.to.have.status(500);
})}
else if(status_code==501){
    pm.test("501 ; STATUS : NOT IMPLEMENTED", function () {
    pm.response.to.have.status(501);
})}
else if(status_code==502){
    pm.test("502 ; STATUS : BAD GATEWAY", function () {
    pm.response.to.have.status(502);
})}
else if(status_code==503){
    pm.test("503 ; STATUS : SERVICE UNAVAILABLE", function () {
    pm.response.to.have.status(503);
})}
```
## Get Final Student Details
Tests Script
```

var status_code = pm.response.code
if(status_code==200){
pm.test("200 ; STATUS : SUCCESS", function () {
    pm.response.to.have.status(200);
});
var jsonData = pm.response.json()
pm.test("Validation of Language 1" ,function () {
    pm.expect(jsonData.data.TechnicalDetails[0].language[0]).to.eql(pm.environment.get("language_code_1"));
});
pm.test("Validation of Language 2" ,function () {
    pm.expect(jsonData.data.TechnicalDetails[0].language[1]).to.eql(pm.environment.get("language_code_2"));
});
pm.test("Validation of Year of Experience", function () {
    pm.expect(jsonData.data.TechnicalDetails[0].yearexp).to.eql(pm.environment.get("yearexp"));
});
pm.test("Validation of House Number", function () {
    pm.expect(jsonData.data.Address[0].Permanent_Address.House_Number).to.eql(pm.environment.get("House_Number"));
});
pm.test("Validation of City", function () {
    pm.expect(jsonData.data.Address[0].Permanent_Address.City).to.eql(pm.environment.get("City"));
});
pm.test("Validation of House Country", function () {
    pm.expect(jsonData.data.Address[0].Permanent_Address.Country).to.eql(pm.environment.get("Country"));
});
pm.test("Validation of Mobile No. 1" ,function () {
    pm.expect(jsonData.data.Address[0].Permanent_Address.PhoneNumber[0].Mobile).to.eql(pm.environment.get("Mobile"));
});
pm.test("Validation of Mobile No. 2" ,function () {
    pm.expect(jsonData.data.Address[0].Permanent_Address.PhoneNumber[1].Mobile).to.eql(pm.environment.get("Mobile_1"));
});
pm.test("Validation of Current Address" ,function () {
    pm.expect(jsonData.data.Address[0].Current_Address).to.eql(null);
});
}
else if(status_code==201){
    pm.test("201 ; STATUS : CREATED", function () {
    pm.response.to.have.status(201);
})}
else if(status_code==202){
    pm.test("202 ; STATUS : ACCEPTED", function () {
    pm.response.to.have.status(202);
})}
else if(status_code==400){
    pm.test("400 ; STATUS : BAD REQUEST", function () {
    pm.response.to.have.status(400);
})}
else if(status_code==401){
    pm.test("401 ; STATUS : UNAUTHORIZED", function () {
    pm.response.to.have.status(401);
})}
else if(status_code==402){
    pm.test("402 ; STATUS : PAYMENT REQUIRED", function () {
    pm.response.to.have.status(402);
})}
else if(status_code==403){
    pm.test("403 ; STATUS : FORBIDDEN", function () {
    pm.response.to.have.status(403);
})}
else if(status_code==404){
    pm.test("404 ; STATUS : NOT FOUND", function () {
    pm.response.to.have.status(404);
})}
else if(status_code==405){
    pm.test("405 ; STATUS : METHOD NOT ALLOWED", function () {
    pm.response.to.have.status(405);
})}
else if(status_code==500){
    pm.test("500 ; STATUS : INTERNAL SERVER ERROR", function () {
    pm.response.to.have.status(500);
})}
else if(status_code==501){
    pm.test("501 ; STATUS : NOT IMPLEMENTED", function () {
    pm.response.to.have.status(501);
})}
else if(status_code==502){
    pm.test("502 ; STATUS : BAD GATEWAY", function () {
    pm.response.to.have.status(502);
})}
else if(status_code==503){
    pm.test("503 ; STATUS : SERVICE UNAVAILABLE", function () {
    pm.response.to.have.status(503);
})}
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
newman run CollectionName.json -e EnvironmentName.json -r cli,HTML
```
Or
```
newman run CollectionName.json -e EnvironmentName.json -r cli,htmlextra
```
!![_C__Users_esrat_OneDrive_Desktop_API_newman_Testing-2024-11-19-08-53-02-922-0 html (1)](https://github.com/user-attachments/assets/157b0f0e-03d8-46f0-b353-a46a34bacd9e)


 
