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


## Report Configure

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
![_C__Users_esrat_OneDrive_Desktop_API_newman_Testing-2024-11-19-08-53-02-922-0 html (1)](https://github.com/user-attachments/assets/157b0f0e-03d8-46f0-b353-a46a34bacd9e)

[Uploading Testing-2024-11-19-08-53-02-922-0.html…]()<!DOCTYPE html>
<html lang="en" style="overflow-y: scroll;">
<head>
  <meta charset="UTF-8">
  <title>Newman Summary Report</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.13.0/css/all.min.css">
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/10.1.2/styles/default.min.css">
  <link rel="stylesheet" href="https://cdn.datatables.net/v/bs4/dt-1.10.18/datatables.min.css"/>

<style>
code.renderMarkdown table, code.renderMarkdown th, code.renderMarkdown td {
    border: 1px solid black;
    width: max-content;
    padding: .75rem;
}

.theme-dark {
    --background-color: #222;
    --bg-card-deck: #444444;
    --text-color: #ccd2d8;
    --tab-border: solid 1px #444;
    --success: #3c9372;
    --failure: #c24a3f;
    --warning: #d28c23;
    --info: #4083b6;
    --badge-outline: #3c9372;
    --badge-bg: #cdd3db;
    --badge-text: #ccd2d9;
    --failure-row: #c24a3f;
    --warning-row: #d28c23;
    --card-bg: #444;
    --card-footer: #4f5758;
    --form-input: #ececb5;
    --hov-text: #d2dae5;
    --h4-text: #ccd1d9;
}

.theme-light {
    --tab-border: solid 1px #fff;
    --text-color: #000000;
    --success: #42a745;
    --failure: #dc3544;
    --warning: #f4c10b;
    --info: #49a1b8;
    --badge-outline: #040411;
    --badge-bg: #f8f9fa;
    --badge-text: #fff;
    --failure-row: #f5c6cb;
    --warning-row: #ffeeba;
    --card-bg: #f7f7f7;
    --hov-text: #fff;
    --h4-text: #ffffff;
}

body {
  padding-top:30px;
  background-color: var(--background-color)!important;
  color: var(--text-color);
}

#execution-data {
  padding: 10px;
  border: var(--tab-border);
  border-top: none;
}

.nav-tabs {
  padding-top: 10px;
  height: 105px;
  overflow-y: auto;
}

body.theme-dark .card-header {
    background-color: #444;
}

body.theme-light .card-header {
    background-color: #f7f7f7;
}

.card-footer {
    padding: .75rem 1.25rem;
    background-color: var(--card-footer);
}

.card-deck {
    background-color: var(--bg-card-deck)!important;
}
.form-control {
    background: var(--form-input);
}

.custom-tab {
  padding: 10px 15px;
  margin-right: 0px;
  height: 47px;
  width: 69px;
  text-align: center;
  border: var(--tab-border);
  border-bottom: none;
  cursor:pointer;
}

body.theme-dark .text-white {
    color: #ccd2d9!important;
}
h4 {
    color: var(--h4-text);
}

body.theme-dark h1 {
    color: #ccd2da;
}

body.theme-dark .bg-light>td {
    background: #4f5858!important;
}

body.theme-dark .hljs {
    background: #0a0a0ab0!important;
    color: #8d8787!important;
}

.bg-info {
    background-color: var(--info)!important;
}
.bg-success {
    background-color: var(--success)!important;
}

.alert-success {
    background-color: var(--success)!important;
}

.alert-warning {
    background-color: var(--warning)!important;
}

.alert-info {
    background-color: var(--info)!important;
}

.bg-warning {
    background-color: var(--warning)!important;
}

.badge-warning {
    color: var(--badge-text)!important;
    background-color: var(--warning)!important;
}

.table-warning>td {
    background-color: var(--warning-row);
}

.alert-danger {
    background-color: var(--failure)!important;
}

body.theme-dark .alert-dark {
    background-color: #636467!important;
}

body.theme-dark .text-dark {
    color: #d1dae4!important;
}

body.theme-dark .badge-light {
    color: #212529;
    background-color: #cdd3db;
}

body.theme-light .badge-light {
    color: #212529;
    background-color: #f8f9fa;
}
body.theme-light .bg-danger {
    background-color: var(--failure)!important;
}

body.theme-dark .bg-danger {
    background-color: var(--failure)!important;
}

.table-danger>td {
    background-color: var(--failure-row);
}

body.theme-dark .table .thead-light th {
    background-color: #4f5858!important;
    border-color: #dee2e6!important;
    color: #ccd2d8!important;
}

.itPassed {
  background: var(--success);
  color: white;
}
.itFailed {
  background: var(--failure);
  color: white;
}

.resultsInfoPass {
  color: var(--success);
  padding-top: 4px;
}

.resultsInfoFail {
  color: var(--failure);
  padding-top: 4px;
}

.badge-outline-success {
  color: var(--success);
  border: 1px solid var(--success);
  background-color: transparent;
}

.badge-outline {
  color: var(--badge-outline);
  border: 1px solid var(--badge-outline);
  background-color: transparent;
}

.btn-outline-success {
    color: var(--success)!important;
    border-color: var(--success)!important;
}

.backToTop:hover {
  background-color: var(--success);
  border-color: var(--success);
  color: var(--hov-text)!important;
}

.btn-outline-success:hover {
  background-color: var(--success);
  border-color: var(--success);
  color: var(--hov-text)!important;
}

.btn-outline-secondary {
  background-color: var(--success)!important;
  color: var(--hov-text)!important;
}

body.theme-dark .env-heading {
    color: #ccd2d9!important;
}

body, html {
  height:100%;
}

.card {
  overflow:hidden;
}

body.theme-dark .card-body {
    background-color: #444;
}

body.theme-light .card-body {
    background-color: #f7f7f7;
}

body.theme-dark .card-body .bg-danger {
    background-color: var(--failure)!important;
}

body.theme-light .card-body .bg-danger {
    background-color: var(--failure)!important;
}

.card-body .rotate {
  z-index: 8;
  float: right;
  height: 100%;
}

.card-body .rotate i {
  color: #14141426;
  position: absolute;
  left: 0;
  left: auto;
  right: -10px;
  bottom: 0;
  display: block;
  -webkit-transform: rotate(-44deg);
  -moz-transform: rotate(-44deg);
  -o-transform: rotate(-44deg);
  -ms-transform: rotate(-44deg);
  transform: rotate(-44deg);
}

.dyn-height {
  max-height:350px;
  overflow-y:auto;
}

.nav-pills .nav-link.active {
  background-color: transparent!important;
}

.backToTop {
  display: none;
  position: fixed;
  bottom: 10px;
  right: 20px;
  z-index: 99;
  font-size: 15px;
  outline: none;
  cursor: pointer;
  padding: 15px;
  border-radius: 4px;
}

.card-header .fa {
  transition: .3s transform ease-in-out;
}
.card-header .collapsed .fa {
  transform: rotate(90deg);
}

.single-line-tabs {
  padding-top: 10px;
  height: 60px;
}

::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: #888;
}

::-webkit-scrollbar-thumb:hover {
  background: #555;
}

/* The switch - the box around the slider */
.switch {
  position: relative;
  display: inline-block;
  width: 44px;
  height: 20px;
}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 20px;
  width: 20px;
  left: 0px;
  bottom: 4px;
  top: 0;
  bottom: 0;
  margin: auto 0;
  -webkit-transition: 0.4s;
  transition: 0.4s;
  box-shadow: 0 0px 15px #2020203d;
  background: white;
  background-repeat: no-repeat;
  background-position: center;
}

input:checked + .slider {
  background-color: #4083b6;
}

input:focus + .slider {
  box-shadow: 0 0 1px #4083b6;
}

input:checked + .slider:before {
  -webkit-transform: translateX(24px);
  -ms-transform: translateX(24px);
  transform: translateX(24px);
  background: white;
  background-repeat: no-repeat;
  background-position: center;
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}

table.dataTable td, table.dataTable tr {
    vertical-align: middle;
}

body.theme-dark code {
    color: #ccd2d8!important;
}

body.theme-light code {
    color: #000000!important;
}

.text-wrap {
    word-wrap: break-word; 
    min-width: 600px; 
    max-width: 600px; 
    white-space: normal;
}

</style>
</head>
<body class="theme-dark">
<script>
    function setTheme(themeName) {
        localStorage.setItem('theme', themeName);
        document.body.className = themeName;
    }

    function toggleTheme() {
        if (localStorage.getItem('theme') === 'theme-light') {
            setTheme('theme-dark');
        } else {
            setTheme('theme-light');
        }
    }
</script>
  <div class="container">
        <div class="container">
            <label>Light</label>
            <label id="switch" class="switch">
                <input type="checkbox" onchange="toggleTheme()" id="slider">
                <span class="slider round"></span>
            </label>
            <label>Dark</label>
        </div>
        <div class="card">
        <div class="card-header">
            <ul class="nav nav-pills mb-3 nav-justified" id="pills-tab" role="tablist">
            <li class="nav-item bg-info active" data-toggle="tooltip" title="Click to view the Summary">
                <a class="nav-link text-white" id="pills-summary-tab" data-toggle="pill" href="#pills-summary" role="tab" aria-controls="pills-summary" aria-selected="true">Summary</a>
            </li>
            <li class="nav-item bg-success" data-toggle="tooltip" title="Click to view the Requests">
                <a class="nav-link text-white" id="pills-requests-tab" data-toggle="pill" href="#pills-requests" role="tab" aria-controls="pills-requests" aria-selected="false">Total Requests <span class="badge badge-light">6</span></a>
            </li>
            <li class="nav-item bg-danger" data-toggle="tooltip" title="Click to view the Failed Tests">
                <a class="nav-link text-white" id="pills-failed-tab" data-toggle="pill" href="#pills-failed" role="tab" aria-controls="pills-failed" aria-selected="false">Failed Tests <span class="badge badge-light">1</span></a>
            </li>
            <li class="nav-item bg-warning" data-toggle="tooltip" title="Click to view the Skipped Tests">
                <a class="nav-link text-white" id="pills-skipped-tab" data-toggle="pill" href="#pills-skipped" role="tab" aria-controls="pills-skipped" aria-selected="false">Skipped Tests <span class="badge badge-light">0</span></a>
            </li>
            </ul>
        <div class="tab-content" id="pills-tabContent">
            <div class="tab-pane fade show active" id="pills-summary" role="tabcard" aria-labelledby="pills-summary-tab">
<div class="row">
  <div class="col-md-9 col-lg-12 main">
   <h1 class="display-2 text-center">Newman Run Dashboard</h1>
   <h5 class="text-center">Tuesday, 19 November 2024 14:53:02</h5>
   <div class="row">
    <div class="col-lg-3 col-md-6">
     <div class="card text-white card-success">
      <div class="card-body bg-danger">
       <div class="rotate">
        <i class="fa fa-retweet fa-5x"></i>
       </div>
       <h6 class="text-uppercase">Total Iterations</h6>
       <h1 class="display-1">1</h1>
      </div>
     </div>
    </div>
    <div class="col-lg-3 col-md-6">
     <div class="card text-white card-danger">
      <div class="card-body bg-success">
       <div class="rotate">
        <i class="fa fa-list fa-4x"></i>
       </div>
       <h6 class="text-uppercase">Total Assertions</h6>
       <h1 class="display-1">12</h1>
      </div>
     </div>
    </div>
    <div class="col-lg-3 col-md-6">
     <div class="card text-white card-info">
      <div class="card-body bg-danger">
       <div class="rotate">
        <i class="fa fa-plus-circle fa-5x"></i>
       </div>
       <h6 class="text-uppercase">Total Failed Tests</h6>
       <h1 class="display-1">1</h1>
      </div>
     </div>
    </div>
    <div class="col-lg-3 col-md-6">
     <div class="card text-white card-warning">
      <div class="card-body bg-success">
       <div class="rotate">
        <i class="fa fa-share fa-5x"></i>
       </div>
       <h6 class="text-uppercase">Total Skipped Tests</h6>
       <h1 class="display-1">0</h1>
      </div>
     </div>
    </div>
   </div>
   <hr>
    <div class="row">
    <div class="col">
        <div class="row">
        <div class="col-sm-12 mb-3">
            <div class="card border-info">
                <div class="card-body">
                <h5 class="card-title text-uppercase text-white text-center bg-info">File Information</h5>
                <span><i class="fas fa-file-code"></i></span><strong> Collection:</strong> Testing<br>
                
                <span><i class="fas fa-file-code"></i></span><strong> Environment:</strong> Api_Testing<br>
                </div>
            </div>
        </div>
        </div>
        <div class="row">
        <div class="col-sm-12 mb-3">
            <div class="card border-info">
                <div class="card-body">
                <h5 class="card-title text-uppercase text-white text-center bg-info">Timings and Data</h5>
                <span><i class="fas fa-stopwatch"></i></span><strong> Total run duration:</strong> 2.9s<br>
                <span><i class="fas fa-database"></i></span><strong> Total data received:</strong> 759B<br>
                <span><i class="fas fa-stopwatch"></i></span><strong> Average response time:</strong> 394ms<br>
                </div>
            </div>
        </div>
        </div>
        <div class="row">
        <div class="col-sm-12 mb-3">
            <div class="table-responsive">
            <table class="table table-striped table-bordered">
                <thead class="thead-inverse">
                    <tr class="text-center">
                        <th class="text-uppercase">Summary Item</th>
                        <th class="text-uppercase">Total</th>
                        <th class="text-uppercase">Failed</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Requests</td>
                        <td class="text-center">6</td>
                        <td class="text-center">0</td>
                    </tr>
                    <tr>
                        <td>Prerequest Scripts</td>
                        <td class="text-center">5</td>
                        <td class="text-center">0</td>
                    </tr>
                    <tr>
                        <td>Test Scripts</td>
                        <td class="text-center">5</td>
                        <td class="text-center">0</td>
                    </tr>
                    <tr class="table-danger">
                        <td>Assertions</td>
                        <td class="text-center">12</td>
                        <td class="text-center">1</td>
                    </tr>
                    <tr class="">
                        <td>Skipped Tests</td>
                        <td class="text-center">0</td>
                        <td class="text-center">-</td>
                    </tr>
                </tbody>
            </table>
            </div>
        </div>
        </div>
    <hr>
   </div>
   </div>
  </div>
 </div>
        </div>
            <div class="tab-pane fade" id="pills-failed" role="tabcard" aria-labelledby="pills-failed-tab">
                <button id="topOfFailuresScreen" class="btn btn-outline-success btn-sm backToTop" onclick="topFunction()">Go To Top</button>

                    <div class="btn-group float-right" role="group" aria-label="Button Group">
                        <button id="openAllFailed" class="btn btn-outline-success btn-sm float-right" style="text-align: center; width: 185px;">Expand All Failed Tests</button>
                    </div>
                    <br>
                    <br>

                    <div class="alert alert-danger text-uppercase text-center">
                        <h4>Showing 1 Failure</h4>
                    </div>
                    <div class="col-sm-12 mb-3">
                    <div class="card-deck">
                        <div class="card border-danger">
                            <div class="card-header bg-danger">
                                <a data-toggle="collapse" href="#" data-target="#fails-collapse-9bfd556b-3e5a-4cb9-87c3-7a35e3d95f68" aria-expanded="false" aria-controls="collapse" id="fails-9bfd556b-3e5a-4cb9-87c3-7a35e3d95f68" class="collapsed text-white z-block">
                                    Iteration 1 - AssertionError - Testing - Get Booking <i class="float-lg-right fa fa-chevron-down" style="padding-top:5px;"></i>
                                </a>
                            </div>
                            <div id="fails-collapse-9bfd556b-3e5a-4cb9-87c3-7a35e3d95f68" class="collapse" aria-labelledby="fails-9bfd556b-3e5a-4cb9-87c3-7a35e3d95f68">
                            <div class="card-body">
                                <h5 ><strong>Failed Test:</strong> Check out Validation</h5>
                            <hr>
                            <h5 class="card-title text-uppercase text-white text-center bg-danger">Assertion Error Message</h5>
                            <div>
                                <pre><code >expected undefined to deeply equal &#x27;2025-02-22&#x27;</code></pre>
                            </div>
                            </div>
                            </div>
                        </div>
                    </div>
                    </div>
            </div>

            <div class="tab-pane fade" id="pills-skipped" role="tabcard" aria-labelledby="pills-skipped-tab">
                <button id="topOfSkippedScreen" class="btn btn-outline-success btn-sm backToTop" onclick="topFunction()">Go To Top</button>

                <div class="alert alert-success text-uppercase text-center">
                    <br><br><h1 class="text-center">There are no skipped tests <span><i class="far fa-thumbs-up"></i></span></h1><br><br>
                </div>
            </div>
            <div class="tab-pane fade" id="pills-requests" role="tabcard" aria-labelledby="pills-requests-tab">

            <button id="topOfRequestsScreen" class="btn btn-outline-success btn-sm backToTop" onclick="topFunction()">Go To Top</button>

            <div class="btn-group float-right" role="group" aria-label="Button Group">
                <button id="showOnlyFailures" class="btn btn-outline-success btn-sm float-right" style="text-align: center; width:160px;">Show Failed Iterations</button>
                <button id="openAll" class="btn btn-outline-success btn-sm float-right" style="text-align: center; width: 140px;">Expand Folders</button>
                <button id="openAllRequests" class="btn btn-outline-success btn-sm float-right" style="text-align: center; width: 140px;">Expand Requests</button>
            </div>

    <div class="text-uppercase" id="execution-menu">
        <h5>1 Iteration available to view</h5>
        
        <nav class="table-responsive">
        <ul class="nav single-line-tabs" id="iterationList">
        </ul>
        </nav>
    </div>
    <h6 class="text-uppercase text-muted" id="iterationSelected" style="padding-top: 10px;"></h6>
	<div class="tab-content" id="execution-data">
            <div id="folder-dab02eae-1019-44cf-804f-9298ec59065d" class="card-deck iteration-0">
            <div class="row iteration-0">
                <div class="col-sm-12 mb-3 iteration-0">
                <div class="card iteration-0">
                    <div class="card-header  bg-success iteration-0">
                        <a data-toggle="collapse" href="#" data-target="#collapse-dab02eae-1019-44cf-804f-9298ec59065d" aria-expanded="false" aria-controls="collapse" id="requests-dab02eae-1019-44cf-804f-9298ec59065d" class="collapsed text-white z-block">
                    Iteration: 1 - Create Booking <i class="float-lg-right fa fa-chevron-down" style="padding-top: 5px;"></i>
                </a>
                    </div>
                    <div id="collapse-dab02eae-1019-44cf-804f-9298ec59065d" class="collapse" aria-labelledby="requests-dab02eae-1019-44cf-804f-9298ec59065d">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Request Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request Method:</strong> <span class="badge-outline-success badge badge-success"> POST</span><br>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request URL:</strong> <a href="https://restful-booker.herokuapp.com/booking/" target="_blank">https://restful-booker.herokuapp.com/booking/</a><br>
                                    </div>
                                </div>
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Response Code:</strong> <span class="float-right badge-outline badge badge-success"> 200 - OK</span><br>
                                    <span><i class="fas fa-stopwatch"></i></span><strong> Mean time per request:</strong> <span class="float-right badge-outline badge badge-success"> 1171ms</span><br>
                                    <span><i class="fas fa-database"></i></span><strong> Mean size per request:</strong> <span class="float-right badge-outline badge badge-success"> 203B</span><br>
                                    <hr>
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Test Pass Percentage</h5>
                                    <div>
                                        <div class="progress" style="height: 40px;">
                                            <div class="progress-bar bg-success" style="width: 100%" role="progressbar">
                                            <h5 class="text-uppercase text-white text-center" style="padding-top:5px;"><strong>No Tests for this request</strong></h5>
                                            </div>
                                        </div>
                                    </div>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                        <h5 class="card-title text-uppercase text-white text-center bg-info">Request Headers</h5>
                                        <div class="table-responsive">
                                            <table class="table table-bordered">
                                            <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                                <tbody>
                                                    <tr>
                                                        <td class="text-nowrap">Content-Type</td>
                                                        <td class="text-wrap">application/json</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">User-Agent</td>
                                                        <td class="text-wrap">PostmanRuntime/7.39.1</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept</td>
                                                        <td class="text-wrap">*/*</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Cache-Control</td>
                                                        <td class="text-wrap">no-cache</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Postman-Token</td>
                                                        <td class="text-wrap">edb87a11-2b02-480d-9952-24774efc7119</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Host</td>
                                                        <td class="text-wrap">restful-booker.herokuapp.com</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept-Encoding</td>
                                                        <td class="text-wrap">gzip, deflate, br</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Connection</td>
                                                        <td class="text-wrap">keep-alive</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Content-Length</td>
                                                        <td class="text-wrap">229</td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                    </div>
                                </div>
                                </div>
                            </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Request Body</h5>
                                        <div class="dyn-height">
                                            <pre><code id="copyReqText-0" class="prettyPrint">{
        	&quot;firstname&quot; : &quot;Aaron&quot;,
        	&quot;lastname&quot; : &quot;O&#x27;Keefe&quot;,
        	&quot;totalprice&quot; : 529,
        	&quot;depositpaid&quot; : true,
        	&quot;bookingdates&quot; : {
            	&quot;checkin&quot; : &quot;2025-02-20&quot;,
            	&quot;checkout&quot; : &quot;2025-02-22&quot;
        	},
        	&quot;additionalneeds&quot; : &quot;application&quot;
        }
        </code></pre>
                                        </div>
                                        <div class="card-footer">
                                            <button class="btn btn-outline-secondary btn-sm copyButton" type="button" data-clipboard-action="copy" data-clipboard-target="#copyReqText-0">Copy to Clipboard</button>
                                        </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Headers</h5>
                                    <div class="table-responsive">
                                        <table class="table table-bordered">
                                        <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                            <tbody>
                                                <tr>
                                                    <td class="text-nowrap">Server</td>
                                                    <td class="text-wrap">Cowboy</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Report-To</td>
                                                    <td class="text-wrap">{&quot;group&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?ts&#x3D;1732006381&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;BNff7Vhpk%2BhRMmBXPtiTJ2ivjqltPcDFlyj2RuOPkWo%3D&quot;}]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Reporting-Endpoints</td>
                                                    <td class="text-wrap">heroku-nel&#x3D;https://nel.heroku.com/reports?ts&#x3D;1732006381&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;BNff7Vhpk%2BhRMmBXPtiTJ2ivjqltPcDFlyj2RuOPkWo%3D</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Nel</td>
                                                    <td class="text-wrap">{&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.005,&quot;failure_fraction&quot;:0.05,&quot;response_headers&quot;:[&quot;Via&quot;]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Connection</td>
                                                    <td class="text-wrap">keep-alive</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">X-Powered-By</td>
                                                    <td class="text-wrap">Express</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Type</td>
                                                    <td class="text-wrap">application/json; charset&#x3D;utf-8</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Length</td>
                                                    <td class="text-wrap">203</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Etag</td>
                                                    <td class="text-wrap">W/&quot;cb-A5PhPNeHdoyds/8aKQXBfW202K0&quot;</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Date</td>
                                                    <td class="text-wrap">Tue, 19 Nov 2024 08:53:01 GMT</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Via</td>
                                                    <td class="text-wrap">1.1 vegur</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Body</h5>
                                        <div class="dyn-height">
                                                <pre><code id="copyText-80687e1b-2f32-4c3b-8a8c-80636ee0c529" class="prettyPrint">{&quot;bookingid&quot;:1319,&quot;booking&quot;:{&quot;firstname&quot;:&quot;Aaron&quot;,&quot;lastname&quot;:&quot;O&#x27;Keefe&quot;,&quot;totalprice&quot;:529,&quot;depositpaid&quot;:true,&quot;bookingdates&quot;:{&quot;checkin&quot;:&quot;2025-02-20&quot;,&quot;checkout&quot;:&quot;2025-02-22&quot;},&quot;additionalneeds&quot;:&quot;application&quot;}}</code></pre>
                                        </div>
                                        <div class="card-footer">
                                            <button class="btn btn-outline-secondary btn-sm copyButton" type="button" data-clipboard-action="copy" data-clipboard-target="#copyText-80687e1b-2f32-4c3b-8a8c-80636ee0c529">Copy to Clipboard</button>
                                        </div>
                                </div>
                            </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="card border-info">
                                <div class="card-body">
                                <h5 class="card-title text-uppercase text-white text-center bg-info">Test Information</h5>
                                    <h5 class="alert alert-success text-uppercase text-center">No Tests for this request</h5>
                                </div>
                            </div>
                        </div>
                    </div>
                    </div>
                </div>
                </div>
            </div>
            </div>
            <div id="folder-81605c50-fe5e-43cd-8fb8-2f64757e07e0" class="card-deck iteration-0">
            <div class="row iteration-0">
                <div class="col-sm-12 mb-3 iteration-0">
                <div class="card iteration-0">
                    <div class="card-header  bg-danger iteration-0">
                        <a data-toggle="collapse" href="#" data-target="#collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0" aria-expanded="false" aria-controls="collapse" id="requests-81605c50-fe5e-43cd-8fb8-2f64757e07e0" class="collapsed text-white z-block">
                    Iteration: 1 - Get Booking <i class="float-lg-right fa fa-chevron-down" style="padding-top: 5px;"></i>
                </a>
                    </div>
                    <div id="collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0" class="collapse" aria-labelledby="requests-81605c50-fe5e-43cd-8fb8-2f64757e07e0">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Request Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request Method:</strong> <span class="badge-outline-success badge badge-success"> GET</span><br>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request URL:</strong> <a href="https://restful-booker.herokuapp.com/booking/1319" target="_blank">https://restful-booker.herokuapp.com/booking/1319</a><br>
                                    </div>
                                </div>
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Response Code:</strong> <span class="float-right badge-outline badge badge-success"> 200 - OK</span><br>
                                    <span><i class="fas fa-stopwatch"></i></span><strong> Mean time per request:</strong> <span class="float-right badge-outline badge badge-success"> 241ms</span><br>
                                    <span><i class="fas fa-database"></i></span><strong> Mean size per request:</strong> <span class="float-right badge-outline badge badge-success"> 174B</span><br>
                                    <hr>
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Test Pass Percentage</h5>
                                    <div>
                                        <div class="progress" style="height: 40px;">
                                            <div class="progress-bar  bg-danger" style="width: 100%" role="progressbar">
                                            <h5 style="padding-top:5px;"><strong>83 %</strong></h5>
                                            </div>
                                        </div>
                                    </div>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                        <h5 class="card-title text-uppercase text-white text-center bg-info">Request Headers</h5>
                                        <div class="table-responsive">
                                            <table class="table table-bordered">
                                            <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                                <tbody>
                                                    <tr>
                                                        <td class="text-nowrap">User-Agent</td>
                                                        <td class="text-wrap">PostmanRuntime/7.39.1</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept</td>
                                                        <td class="text-wrap">*/*</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Cache-Control</td>
                                                        <td class="text-wrap">no-cache</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Postman-Token</td>
                                                        <td class="text-wrap">ffd3d953-cb78-465d-a5e9-30ee9463e85c</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Host</td>
                                                        <td class="text-wrap">restful-booker.herokuapp.com</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept-Encoding</td>
                                                        <td class="text-wrap">gzip, deflate, br</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Connection</td>
                                                        <td class="text-wrap">keep-alive</td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                    </div>
                                </div>
                                </div>
                            </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Headers</h5>
                                    <div class="table-responsive">
                                        <table class="table table-bordered">
                                        <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                            <tbody>
                                                <tr>
                                                    <td class="text-nowrap">Server</td>
                                                    <td class="text-wrap">Cowboy</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Report-To</td>
                                                    <td class="text-wrap">{&quot;group&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?ts&#x3D;1732006381&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;BNff7Vhpk%2BhRMmBXPtiTJ2ivjqltPcDFlyj2RuOPkWo%3D&quot;}]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Reporting-Endpoints</td>
                                                    <td class="text-wrap">heroku-nel&#x3D;https://nel.heroku.com/reports?ts&#x3D;1732006381&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;BNff7Vhpk%2BhRMmBXPtiTJ2ivjqltPcDFlyj2RuOPkWo%3D</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Nel</td>
                                                    <td class="text-wrap">{&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.005,&quot;failure_fraction&quot;:0.05,&quot;response_headers&quot;:[&quot;Via&quot;]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Connection</td>
                                                    <td class="text-wrap">keep-alive</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">X-Powered-By</td>
                                                    <td class="text-wrap">Express</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Type</td>
                                                    <td class="text-wrap">application/json; charset&#x3D;utf-8</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Length</td>
                                                    <td class="text-wrap">174</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Etag</td>
                                                    <td class="text-wrap">W/&quot;ae-dWyevkfr3310Ah/3edSS5oBF9bE&quot;</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Date</td>
                                                    <td class="text-wrap">Tue, 19 Nov 2024 08:53:01 GMT</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Via</td>
                                                    <td class="text-wrap">1.1 vegur</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Body</h5>
                                        <div class="dyn-height">
                                                <pre><code id="copyText-c6166958-c974-4339-84a0-e85961b2e524" class="prettyPrint">{&quot;firstname&quot;:&quot;Aaron&quot;,&quot;lastname&quot;:&quot;O&#x27;Keefe&quot;,&quot;totalprice&quot;:529,&quot;depositpaid&quot;:true,&quot;bookingdates&quot;:{&quot;checkin&quot;:&quot;2025-02-20&quot;,&quot;checkout&quot;:&quot;2025-02-22&quot;},&quot;additionalneeds&quot;:&quot;application&quot;}</code></pre>
                                        </div>
                                        <div class="card-footer">
                                            <button class="btn btn-outline-secondary btn-sm copyButton" type="button" data-clipboard-action="copy" data-clipboard-target="#copyText-c6166958-c974-4339-84a0-e85961b2e524">Copy to Clipboard</button>
                                        </div>
                                </div>
                            </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="card border-info">
                                <div class="card-body">
                                <h5 class="card-title text-uppercase text-white text-center bg-info">Test Information</h5>
                                    <div class="table-responsive text-nowrap">
                                        <table class="datatable table table-hover">
                                        <thead><tr class="text-center"><th>Name</th><th>Passed</th><th>Failed</th><th>Skipped</th></tr></thead>
                                            <tbody>
                                                <tr >
                                                    <td >First Name Validation</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Last Name Validation</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Total price Check</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Deposit Paid Validation</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Check in Validation</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Check out Validation</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center bg-danger">1</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                            </tbody>
                                            <tfoot>
                                                <tr class="bg-light">
                                                    <td><strong>Total</strong></td>
                                                    <td class="text-center">5</td>
                                                    <td class="text-center">1</td>
                                                    <td class="text-center">0</td>
                                                </tr>
                                            </tfoot>
                                        </table>
                                    </div>
                                    <div class="row ">
                                    <div class="col-sm-12 mb-3">
                                        <div class="card-deck">
                                        <div class="card border-danger" style="width: 50rem;">
                                            <div class="card-body">
                                                <h5 class="card-title text-uppercase text-white text-center bg-danger">Test Failure</h5>
                                                <div class="table-responsive">
                                                    <table class="table table-hover">
                                                    <thead><tr class="text-nowrap"><th>Test Name</th><th>Assertion Error</th></tr></thead>
                                                        <tbody>
                                                            <tr>
                                                                <td class="w-45 text-nowrap ">Check out Validation</td>
                                                                <td class="w-55"><pre><code >expected undefined to deeply equal &#x27;2025-02-22&#x27;</code></pre></td>
                                                            </tr>
                                                        </tbody>
                                                    </table>
                                                </div>
                                            </div>
                                        </div>
                                        </div>
                                    </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    </div>
                </div>
                </div>
            </div>
            </div>
            <div id="folder-16d31790-216d-4738-a31f-7d04524ac04c" class="card-deck iteration-0">
            <div class="row iteration-0">
                <div class="col-sm-12 mb-3 iteration-0">
                <div class="card iteration-0">
                    <div class="card-header  bg-success iteration-0">
                        <a data-toggle="collapse" href="#" data-target="#collapse-16d31790-216d-4738-a31f-7d04524ac04c" aria-expanded="false" aria-controls="collapse" id="requests-16d31790-216d-4738-a31f-7d04524ac04c" class="collapsed text-white z-block">
                    Iteration: 1 - Token <i class="float-lg-right fa fa-chevron-down" style="padding-top: 5px;"></i>
                </a>
                    </div>
                    <div id="collapse-16d31790-216d-4738-a31f-7d04524ac04c" class="collapse" aria-labelledby="requests-16d31790-216d-4738-a31f-7d04524ac04c">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Request Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request Method:</strong> <span class="badge-outline-success badge badge-success"> POST</span><br>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request URL:</strong> <a href="https://restful-booker.herokuapp.com/auth" target="_blank">https://restful-booker.herokuapp.com/auth</a><br>
                                    </div>
                                </div>
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Response Code:</strong> <span class="float-right badge-outline badge badge-success"> 200 - OK</span><br>
                                    <span><i class="fas fa-stopwatch"></i></span><strong> Mean time per request:</strong> <span class="float-right badge-outline badge badge-success"> 239ms</span><br>
                                    <span><i class="fas fa-database"></i></span><strong> Mean size per request:</strong> <span class="float-right badge-outline badge badge-success"> 27B</span><br>
                                    <hr>
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Test Pass Percentage</h5>
                                    <div>
                                        <div class="progress" style="height: 40px;">
                                            <div class="progress-bar bg-success" style="width: 100%" role="progressbar">
                                            <h5 class="text-uppercase text-white text-center" style="padding-top:5px;"><strong>No Tests for this request</strong></h5>
                                            </div>
                                        </div>
                                    </div>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                        <h5 class="card-title text-uppercase text-white text-center bg-info">Request Headers</h5>
                                        <div class="table-responsive">
                                            <table class="table table-bordered">
                                            <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                                <tbody>
                                                    <tr>
                                                        <td class="text-nowrap">Content-Type</td>
                                                        <td class="text-wrap">application/json</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">User-Agent</td>
                                                        <td class="text-wrap">PostmanRuntime/7.39.1</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept</td>
                                                        <td class="text-wrap">*/*</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Cache-Control</td>
                                                        <td class="text-wrap">no-cache</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Postman-Token</td>
                                                        <td class="text-wrap">8ae20cbf-7c5c-4b02-9298-af73fdafc559</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Host</td>
                                                        <td class="text-wrap">restful-booker.herokuapp.com</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept-Encoding</td>
                                                        <td class="text-wrap">gzip, deflate, br</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Connection</td>
                                                        <td class="text-wrap">keep-alive</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Content-Length</td>
                                                        <td class="text-wrap">57</td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                    </div>
                                </div>
                                </div>
                            </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Request Body</h5>
                                        <div class="dyn-height">
                                            <pre><code id="copyReqText-2" class="prettyPrint">{
        	&quot;username&quot;: &quot;admin&quot;,
        	&quot;password&quot;: &quot;password123&quot;
        }
        </code></pre>
                                        </div>
                                        <div class="card-footer">
                                            <button class="btn btn-outline-secondary btn-sm copyButton" type="button" data-clipboard-action="copy" data-clipboard-target="#copyReqText-2">Copy to Clipboard</button>
                                        </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Headers</h5>
                                    <div class="table-responsive">
                                        <table class="table table-bordered">
                                        <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                            <tbody>
                                                <tr>
                                                    <td class="text-nowrap">Server</td>
                                                    <td class="text-wrap">Cowboy</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Report-To</td>
                                                    <td class="text-wrap">{&quot;group&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?ts&#x3D;1732006381&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;BNff7Vhpk%2BhRMmBXPtiTJ2ivjqltPcDFlyj2RuOPkWo%3D&quot;}]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Reporting-Endpoints</td>
                                                    <td class="text-wrap">heroku-nel&#x3D;https://nel.heroku.com/reports?ts&#x3D;1732006381&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;BNff7Vhpk%2BhRMmBXPtiTJ2ivjqltPcDFlyj2RuOPkWo%3D</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Nel</td>
                                                    <td class="text-wrap">{&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.005,&quot;failure_fraction&quot;:0.05,&quot;response_headers&quot;:[&quot;Via&quot;]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Connection</td>
                                                    <td class="text-wrap">keep-alive</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">X-Powered-By</td>
                                                    <td class="text-wrap">Express</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Type</td>
                                                    <td class="text-wrap">application/json; charset&#x3D;utf-8</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Length</td>
                                                    <td class="text-wrap">27</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Etag</td>
                                                    <td class="text-wrap">W/&quot;1b-a4p1SMhgO70tvEYXW/s+o/43UaU&quot;</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Date</td>
                                                    <td class="text-wrap">Tue, 19 Nov 2024 08:53:01 GMT</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Via</td>
                                                    <td class="text-wrap">1.1 vegur</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Body</h5>
                                        <div class="dyn-height">
                                                <pre><code id="copyText-34ef6200-edc5-4327-91ee-0665805afd0e" class="prettyPrint">{&quot;token&quot;:&quot;fc8b39cf694f354&quot;}</code></pre>
                                        </div>
                                        <div class="card-footer">
                                            <button class="btn btn-outline-secondary btn-sm copyButton" type="button" data-clipboard-action="copy" data-clipboard-target="#copyText-34ef6200-edc5-4327-91ee-0665805afd0e">Copy to Clipboard</button>
                                        </div>
                                </div>
                            </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="card border-info">
                                <div class="card-body">
                                <h5 class="card-title text-uppercase text-white text-center bg-info">Test Information</h5>
                                    <h5 class="alert alert-success text-uppercase text-center">No Tests for this request</h5>
                                </div>
                            </div>
                        </div>
                    </div>
                    </div>
                </div>
                </div>
            </div>
            </div>
            <div id="folder-0be779c4-b45c-4f71-bd27-f45ee377e00a" class="card-deck iteration-0">
            <div class="row iteration-0">
                <div class="col-sm-12 mb-3 iteration-0">
                <div class="card iteration-0">
                    <div class="card-header  bg-success iteration-0">
                        <a data-toggle="collapse" href="#" data-target="#collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a" aria-expanded="false" aria-controls="collapse" id="requests-0be779c4-b45c-4f71-bd27-f45ee377e00a" class="collapsed text-white z-block">
                    Iteration: 1 - Update Booking <i class="float-lg-right fa fa-chevron-down" style="padding-top: 5px;"></i>
                </a>
                    </div>
                    <div id="collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a" class="collapse" aria-labelledby="requests-0be779c4-b45c-4f71-bd27-f45ee377e00a">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Request Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request Method:</strong> <span class="badge-outline-success badge badge-success"> PUT</span><br>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request URL:</strong> <a href="https://restful-booker.herokuapp.com/booking/1319" target="_blank">https://restful-booker.herokuapp.com/booking/1319</a><br>
                                    </div>
                                </div>
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Response Code:</strong> <span class="float-right badge-outline badge badge-success"> 200 - OK</span><br>
                                    <span><i class="fas fa-stopwatch"></i></span><strong> Mean time per request:</strong> <span class="float-right badge-outline badge badge-success"> 236ms</span><br>
                                    <span><i class="fas fa-database"></i></span><strong> Mean size per request:</strong> <span class="float-right badge-outline badge badge-success"> 173B</span><br>
                                    <hr>
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Test Pass Percentage</h5>
                                    <div>
                                        <div class="progress" style="height: 40px;">
                                            <div class="progress-bar bg-success" style="width: 100%" role="progressbar">
                                            <h5 class="text-uppercase text-white text-center" style="padding-top:5px;"><strong>No Tests for this request</strong></h5>
                                            </div>
                                        </div>
                                    </div>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                        <h5 class="card-title text-uppercase text-white text-center bg-info">Request Headers</h5>
                                        <div class="table-responsive">
                                            <table class="table table-bordered">
                                            <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                                <tbody>
                                                    <tr>
                                                        <td class="text-nowrap">Cookie</td>
                                                        <td class="text-wrap">token&#x3D;fc8b39cf694f354</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Content-Type</td>
                                                        <td class="text-wrap">application/json</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">User-Agent</td>
                                                        <td class="text-wrap">PostmanRuntime/7.39.1</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept</td>
                                                        <td class="text-wrap">*/*</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Cache-Control</td>
                                                        <td class="text-wrap">no-cache</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Postman-Token</td>
                                                        <td class="text-wrap">5e00970b-feb7-42fb-9f4a-020b221fcd17</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Host</td>
                                                        <td class="text-wrap">restful-booker.herokuapp.com</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept-Encoding</td>
                                                        <td class="text-wrap">gzip, deflate, br</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Connection</td>
                                                        <td class="text-wrap">keep-alive</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Content-Length</td>
                                                        <td class="text-wrap">228</td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                    </div>
                                </div>
                                </div>
                            </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Request Body</h5>
                                        <div class="dyn-height">
                                            <pre><code id="copyReqText-3" class="prettyPrint">{
        	&quot;firstname&quot; : &quot;Jeramie&quot;,
        	&quot;lastname&quot; : &quot;Lowe&quot;,
        	&quot;totalprice&quot; : 430,
        	&quot;depositpaid&quot; : true,
        	&quot;bookingdates&quot; : {
            	&quot;checkin&quot; : &quot;2025-02-20&quot;,
            	&quot;checkout&quot; : &quot;2025-02-22&quot;
        	},
        	&quot;additionalneeds&quot; : &quot;application&quot;
        }
        </code></pre>
                                        </div>
                                        <div class="card-footer">
                                            <button class="btn btn-outline-secondary btn-sm copyButton" type="button" data-clipboard-action="copy" data-clipboard-target="#copyReqText-3">Copy to Clipboard</button>
                                        </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Headers</h5>
                                    <div class="table-responsive">
                                        <table class="table table-bordered">
                                        <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                            <tbody>
                                                <tr>
                                                    <td class="text-nowrap">Server</td>
                                                    <td class="text-wrap">Cowboy</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Report-To</td>
                                                    <td class="text-wrap">{&quot;group&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?ts&#x3D;1732006382&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;ZqB9zUx%2BCzxg6AO99nGWWN6Qiwm05ximFiA1zwvBRXQ%3D&quot;}]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Reporting-Endpoints</td>
                                                    <td class="text-wrap">heroku-nel&#x3D;https://nel.heroku.com/reports?ts&#x3D;1732006382&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;ZqB9zUx%2BCzxg6AO99nGWWN6Qiwm05ximFiA1zwvBRXQ%3D</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Nel</td>
                                                    <td class="text-wrap">{&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.005,&quot;failure_fraction&quot;:0.05,&quot;response_headers&quot;:[&quot;Via&quot;]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Connection</td>
                                                    <td class="text-wrap">keep-alive</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">X-Powered-By</td>
                                                    <td class="text-wrap">Express</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Type</td>
                                                    <td class="text-wrap">application/json; charset&#x3D;utf-8</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Length</td>
                                                    <td class="text-wrap">173</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Etag</td>
                                                    <td class="text-wrap">W/&quot;ad-M+AAdXa9dkyMLkQ7vMAUWN6j8Tw&quot;</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Date</td>
                                                    <td class="text-wrap">Tue, 19 Nov 2024 08:53:02 GMT</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Via</td>
                                                    <td class="text-wrap">1.1 vegur</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Body</h5>
                                        <div class="dyn-height">
                                                <pre><code id="copyText-e97cd02a-af28-4b72-bc43-05e3c7fe9fca" class="prettyPrint">{&quot;firstname&quot;:&quot;Jeramie&quot;,&quot;lastname&quot;:&quot;Lowe&quot;,&quot;totalprice&quot;:430,&quot;depositpaid&quot;:true,&quot;bookingdates&quot;:{&quot;checkin&quot;:&quot;2025-02-20&quot;,&quot;checkout&quot;:&quot;2025-02-22&quot;},&quot;additionalneeds&quot;:&quot;application&quot;}</code></pre>
                                        </div>
                                        <div class="card-footer">
                                            <button class="btn btn-outline-secondary btn-sm copyButton" type="button" data-clipboard-action="copy" data-clipboard-target="#copyText-e97cd02a-af28-4b72-bc43-05e3c7fe9fca">Copy to Clipboard</button>
                                        </div>
                                </div>
                            </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="card border-info">
                                <div class="card-body">
                                <h5 class="card-title text-uppercase text-white text-center bg-info">Test Information</h5>
                                    <h5 class="alert alert-success text-uppercase text-center">No Tests for this request</h5>
                                </div>
                            </div>
                        </div>
                    </div>
                    </div>
                </div>
                </div>
            </div>
            </div>
            <div id="folder-69fd433f-b74e-4dba-833c-75508e4a9918" class="card-deck iteration-0">
            <div class="row iteration-0">
                <div class="col-sm-12 mb-3 iteration-0">
                <div class="card iteration-0">
                    <div class="card-header  bg-success iteration-0">
                        <a data-toggle="collapse" href="#" data-target="#collapse-69fd433f-b74e-4dba-833c-75508e4a9918" aria-expanded="false" aria-controls="collapse" id="requests-69fd433f-b74e-4dba-833c-75508e4a9918" class="collapsed text-white z-block">
                    Iteration: 1 - Verify After Update Booking <i class="float-lg-right fa fa-chevron-down" style="padding-top: 5px;"></i>
                </a>
                    </div>
                    <div id="collapse-69fd433f-b74e-4dba-833c-75508e4a9918" class="collapse" aria-labelledby="requests-69fd433f-b74e-4dba-833c-75508e4a9918">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Request Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request Method:</strong> <span class="badge-outline-success badge badge-success"> GET</span><br>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request URL:</strong> <a href="https://restful-booker.herokuapp.com/booking/1319" target="_blank">https://restful-booker.herokuapp.com/booking/1319</a><br>
                                    </div>
                                </div>
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Response Code:</strong> <span class="float-right badge-outline badge badge-success"> 200 - OK</span><br>
                                    <span><i class="fas fa-stopwatch"></i></span><strong> Mean time per request:</strong> <span class="float-right badge-outline badge badge-success"> 240ms</span><br>
                                    <span><i class="fas fa-database"></i></span><strong> Mean size per request:</strong> <span class="float-right badge-outline badge badge-success"> 173B</span><br>
                                    <hr>
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Test Pass Percentage</h5>
                                    <div>
                                        <div class="progress" style="height: 40px;">
                                            <div class="progress-bar  bg-success" style="width: 100%" role="progressbar">
                                            <h5 style="padding-top:5px;"><strong>100 %</strong></h5>
                                            </div>
                                        </div>
                                    </div>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                        <h5 class="card-title text-uppercase text-white text-center bg-info">Request Headers</h5>
                                        <div class="table-responsive">
                                            <table class="table table-bordered">
                                            <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                                <tbody>
                                                    <tr>
                                                        <td class="text-nowrap">User-Agent</td>
                                                        <td class="text-wrap">PostmanRuntime/7.39.1</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept</td>
                                                        <td class="text-wrap">*/*</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Cache-Control</td>
                                                        <td class="text-wrap">no-cache</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Postman-Token</td>
                                                        <td class="text-wrap">8b33de03-ac53-482f-9465-d85e968f1527</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Host</td>
                                                        <td class="text-wrap">restful-booker.herokuapp.com</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept-Encoding</td>
                                                        <td class="text-wrap">gzip, deflate, br</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Connection</td>
                                                        <td class="text-wrap">keep-alive</td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                    </div>
                                </div>
                                </div>
                            </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Headers</h5>
                                    <div class="table-responsive">
                                        <table class="table table-bordered">
                                        <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                            <tbody>
                                                <tr>
                                                    <td class="text-nowrap">Server</td>
                                                    <td class="text-wrap">Cowboy</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Report-To</td>
                                                    <td class="text-wrap">{&quot;group&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?ts&#x3D;1732006382&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;ZqB9zUx%2BCzxg6AO99nGWWN6Qiwm05ximFiA1zwvBRXQ%3D&quot;}]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Reporting-Endpoints</td>
                                                    <td class="text-wrap">heroku-nel&#x3D;https://nel.heroku.com/reports?ts&#x3D;1732006382&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;ZqB9zUx%2BCzxg6AO99nGWWN6Qiwm05ximFiA1zwvBRXQ%3D</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Nel</td>
                                                    <td class="text-wrap">{&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.005,&quot;failure_fraction&quot;:0.05,&quot;response_headers&quot;:[&quot;Via&quot;]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Connection</td>
                                                    <td class="text-wrap">keep-alive</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">X-Powered-By</td>
                                                    <td class="text-wrap">Express</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Type</td>
                                                    <td class="text-wrap">application/json; charset&#x3D;utf-8</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Length</td>
                                                    <td class="text-wrap">173</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Etag</td>
                                                    <td class="text-wrap">W/&quot;ad-M+AAdXa9dkyMLkQ7vMAUWN6j8Tw&quot;</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Date</td>
                                                    <td class="text-wrap">Tue, 19 Nov 2024 08:53:02 GMT</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Via</td>
                                                    <td class="text-wrap">1.1 vegur</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Body</h5>
                                        <div class="dyn-height">
                                                <pre><code id="copyText-020519e2-5dfb-4642-b328-76beab50af59" class="prettyPrint">{&quot;firstname&quot;:&quot;Jeramie&quot;,&quot;lastname&quot;:&quot;Lowe&quot;,&quot;totalprice&quot;:430,&quot;depositpaid&quot;:true,&quot;bookingdates&quot;:{&quot;checkin&quot;:&quot;2025-02-20&quot;,&quot;checkout&quot;:&quot;2025-02-22&quot;},&quot;additionalneeds&quot;:&quot;application&quot;}</code></pre>
                                        </div>
                                        <div class="card-footer">
                                            <button class="btn btn-outline-secondary btn-sm copyButton" type="button" data-clipboard-action="copy" data-clipboard-target="#copyText-020519e2-5dfb-4642-b328-76beab50af59">Copy to Clipboard</button>
                                        </div>
                                </div>
                            </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="card border-info">
                                <div class="card-body">
                                <h5 class="card-title text-uppercase text-white text-center bg-info">Test Information</h5>
                                    <div class="table-responsive text-nowrap">
                                        <table class="datatable table table-hover">
                                        <thead><tr class="text-center"><th>Name</th><th>Passed</th><th>Failed</th><th>Skipped</th></tr></thead>
                                            <tbody>
                                                <tr >
                                                    <td >First Name Validation Update</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Last Name Validation Update</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Total price Check</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Deposit Paid Validation</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Check in Validation</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                                <tr >
                                                    <td >Check out Validation</td>
                                                    <td class="text-center bg-success">1</td>
                                                    <td class="text-center ">0</td>
                                                    <td class="text-center ">0</td>
                                                </tr>
                                            </tbody>
                                            <tfoot>
                                                <tr class="bg-light">
                                                    <td><strong>Total</strong></td>
                                                    <td class="text-center">6</td>
                                                    <td class="text-center">0</td>
                                                    <td class="text-center">0</td>
                                                </tr>
                                            </tfoot>
                                        </table>
                                    </div>
                                    <div class="row d-none">
                                    <div class="col-sm-12 mb-3">
                                        <div class="card-deck">
                                        <div class="card border-danger" style="width: 50rem;">
                                            <div class="card-body">
                                                <h5 class="card-title text-uppercase text-white text-center bg-danger">Test Failure</h5>
                                                <div class="table-responsive">
                                                    <table class="table table-hover">
                                                    <thead><tr class="text-nowrap"><th>Test Name</th><th>Assertion Error</th></tr></thead>
                                                        <tbody>
                                                        </tbody>
                                                    </table>
                                                </div>
                                            </div>
                                        </div>
                                        </div>
                                    </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    </div>
                </div>
                </div>
            </div>
            </div>
            <div id="folder-9c48ff67-bb55-427c-a72c-e25a8f8aadcd" class="card-deck iteration-0">
            <div class="row iteration-0">
                <div class="col-sm-12 mb-3 iteration-0">
                <div class="card iteration-0">
                    <div class="card-header  bg-success iteration-0">
                        <a data-toggle="collapse" href="#" data-target="#collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd" aria-expanded="false" aria-controls="collapse" id="requests-9c48ff67-bb55-427c-a72c-e25a8f8aadcd" class="collapsed text-white z-block">
                    Iteration: 1 - delete booking <i class="float-lg-right fa fa-chevron-down" style="padding-top: 5px;"></i>
                </a>
                    </div>
                    <div id="collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd" class="collapse" aria-labelledby="requests-9c48ff67-bb55-427c-a72c-e25a8f8aadcd">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Request Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request Method:</strong> <span class="badge-outline-success badge badge-success"> DELETE</span><br>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Request URL:</strong> <a href="https://restful-booker.herokuapp.com/booking/1319" target="_blank">https://restful-booker.herokuapp.com/booking/1319</a><br>
                                    </div>
                                </div>
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Information</h5>
                                    <span><i class="fas fa-info-circle"></i></span><strong> Response Code:</strong> <span class="float-right badge-outline badge badge-success"> 403 - Forbidden</span><br>
                                    <span><i class="fas fa-stopwatch"></i></span><strong> Mean time per request:</strong> <span class="float-right badge-outline badge badge-success"> 238ms</span><br>
                                    <span><i class="fas fa-database"></i></span><strong> Mean size per request:</strong> <span class="float-right badge-outline badge badge-success"> 9B</span><br>
                                    <hr>
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Test Pass Percentage</h5>
                                    <div>
                                        <div class="progress" style="height: 40px;">
                                            <div class="progress-bar bg-success" style="width: 100%" role="progressbar">
                                            <h5 class="text-uppercase text-white text-center" style="padding-top:5px;"><strong>No Tests for this request</strong></h5>
                                            </div>
                                        </div>
                                    </div>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-sm-12 mb-3">
                                <div class="card-deck">
                                <div class="card border-info" style="width: 50rem;">
                                    <div class="card-body">
                                        <h5 class="card-title text-uppercase text-white text-center bg-info">Request Headers</h5>
                                        <div class="table-responsive">
                                            <table class="table table-bordered">
                                            <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                                <tbody>
                                                    <tr>
                                                        <td class="text-nowrap">User-Agent</td>
                                                        <td class="text-wrap">PostmanRuntime/7.39.1</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept</td>
                                                        <td class="text-wrap">*/*</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Cache-Control</td>
                                                        <td class="text-wrap">no-cache</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Postman-Token</td>
                                                        <td class="text-wrap">23ebd841-46d7-444b-b904-1c82175b841c</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Host</td>
                                                        <td class="text-wrap">restful-booker.herokuapp.com</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Accept-Encoding</td>
                                                        <td class="text-wrap">gzip, deflate, br</td>
                                                    </tr>
                                                    <tr>
                                                        <td class="text-nowrap">Connection</td>
                                                        <td class="text-wrap">keep-alive</td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                    </div>
                                </div>
                                </div>
                            </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Headers</h5>
                                    <div class="table-responsive">
                                        <table class="table table-bordered">
                                        <thead class="thead-light text-center"><tr><th>Header Name</th><th>Header Value</th></tr></thead>
                                            <tbody>
                                                <tr>
                                                    <td class="text-nowrap">Server</td>
                                                    <td class="text-wrap">Cowboy</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Report-To</td>
                                                    <td class="text-wrap">{&quot;group&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?ts&#x3D;1732006382&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;ZqB9zUx%2BCzxg6AO99nGWWN6Qiwm05ximFiA1zwvBRXQ%3D&quot;}]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Reporting-Endpoints</td>
                                                    <td class="text-wrap">heroku-nel&#x3D;https://nel.heroku.com/reports?ts&#x3D;1732006382&amp;sid&#x3D;c46efe9b-d3d2-4a0c-8c76-bfafa16c5add&amp;s&#x3D;ZqB9zUx%2BCzxg6AO99nGWWN6Qiwm05ximFiA1zwvBRXQ%3D</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Nel</td>
                                                    <td class="text-wrap">{&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.005,&quot;failure_fraction&quot;:0.05,&quot;response_headers&quot;:[&quot;Via&quot;]}</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Connection</td>
                                                    <td class="text-wrap">keep-alive</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">X-Powered-By</td>
                                                    <td class="text-wrap">Express</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Type</td>
                                                    <td class="text-wrap">text/plain; charset&#x3D;utf-8</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Content-Length</td>
                                                    <td class="text-wrap">9</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Etag</td>
                                                    <td class="text-wrap">W/&quot;9-PatfYBLj4Um1qTm5zrukoLhNyPU&quot;</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Date</td>
                                                    <td class="text-wrap">Tue, 19 Nov 2024 08:53:02 GMT</td>
                                                </tr>
                                                <tr>
                                                    <td class="text-nowrap">Via</td>
                                                    <td class="text-wrap">1.1 vegur</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                            </div>
                        </div>
                        </div>
                        <div class="row">
                        <div class="col-sm-12 mb-3">
                            <div class="card-deck">
                            <div class="card border-info" style="width: 50rem;">
                                <div class="card-body">
                                    <h5 class="card-title text-uppercase text-white text-center bg-info">Response Body</h5>
                                        <div class="dyn-height">
                                                <pre><code id="copyText-3dfec524-783c-480a-9158-d21b1e43215f" class="prettyPrint">Forbidden</code></pre>
                                        </div>
                                        <div class="card-footer">
                                            <button class="btn btn-outline-secondary btn-sm copyButton" type="button" data-clipboard-action="copy" data-clipboard-target="#copyText-3dfec524-783c-480a-9158-d21b1e43215f">Copy to Clipboard</button>
                                        </div>
                                </div>
                            </div>
                            </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="card border-info">
                                <div class="card-body">
                                <h5 class="card-title text-uppercase text-white text-center bg-info">Test Information</h5>
                                    <h5 class="alert alert-success text-uppercase text-center">No Tests for this request</h5>
                                </div>
                            </div>
                        </div>
                    </div>
                    </div>
                </div>
                </div>
            </div>
            </div>
        </div>
    </div>
    </div>
    </div>
    </div>
    </div>


<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.2.1/js/bootstrap.bundle.min.js"></script>
<script src="https://cdn.datatables.net/v/bs4/dt-1.10.18/datatables.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/clipboard.js/2.0.0/clipboard.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/remarkable/1.7.1/remarkable.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/10.1.2/highlight.min.js"></script>
<script>hljs.initHighlightingOnLoad();</script>

<!-- Set slider initial position depending on the localstorage state -->

<script>
(function () {
  var sliderChecked = true;
  if (localStorage.getItem('theme') === 'theme-light') {
    setTheme('theme-light');
    sliderChecked = false;
  } else {
    setTheme('theme-dark');
    sliderChecked = true;
  }
  $(document).ready( function () {
    document.getElementById('slider').checked = sliderChecked;
  });
})();
</script>

<!-- Data Table Configuration -->

<script>
$(document).ready( function () {
    $('.datatable').DataTable({
        "retrieve": true,
        "paging": false,
        "info": false,
        "fixedColumns":   {
            "heightMatch": 'none'
        }
    });
});
</script>

<!-- Tooltip Configuration -->

<script>
$(document).ready(function() {
    $('[data-toggle="tooltip"]').tooltip({
        trigger : 'hover'
    })
})
</script>

<!-- Show/Hide The Folders -->

<script>
$('#openAll').on('click', function(e) {
let clickCount = $(this).data("clickCount") || 1
switch (clickCount){
    case 1:
            $('#folder-3d418edc-1db1-497f-927c-25ed933e0797-iteration-0').removeClass('collapsed')
            $('#folder-collapse-3d418edc-1db1-497f-927c-25ed933e0797-iteration-0').addClass('show')
        $('#openAll').html("Collapse Folders");
        break;
    case 2:
            $('#folder-3d418edc-1db1-497f-927c-25ed933e0797-iteration-0').addClass('collapsed')
            $('#folder-collapse-3d418edc-1db1-497f-927c-25ed933e0797-iteration-0').removeClass('show')
        $('#openAll').html("Expand Folders");
        break;
}
clickCount = clickCount > 1 ? 1 : ++clickCount;
$(this).data("clickCount", clickCount)
})
</script>

<!-- Show/Hide The Requests -->

<script>
$('#openAllRequests').on('click', function(e) {
let clickCount = $(this).data("clickCount") || 1
switch (clickCount){
    case 1:
            $('#requests-dab02eae-1019-44cf-804f-9298ec59065d').removeClass('collapsed')
            $('#collapse-dab02eae-1019-44cf-804f-9298ec59065d').removeClass('collapsed')
            $('#requests-dab02eae-1019-44cf-804f-9298ec59065d').addClass('show')
            $('#collapse-dab02eae-1019-44cf-804f-9298ec59065d').addClass('show')
            $('#requests-81605c50-fe5e-43cd-8fb8-2f64757e07e0').removeClass('collapsed')
            $('#collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0').removeClass('collapsed')
            $('#requests-81605c50-fe5e-43cd-8fb8-2f64757e07e0').addClass('show')
            $('#collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0').addClass('show')
            $('#requests-16d31790-216d-4738-a31f-7d04524ac04c').removeClass('collapsed')
            $('#collapse-16d31790-216d-4738-a31f-7d04524ac04c').removeClass('collapsed')
            $('#requests-16d31790-216d-4738-a31f-7d04524ac04c').addClass('show')
            $('#collapse-16d31790-216d-4738-a31f-7d04524ac04c').addClass('show')
            $('#requests-0be779c4-b45c-4f71-bd27-f45ee377e00a').removeClass('collapsed')
            $('#collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a').removeClass('collapsed')
            $('#requests-0be779c4-b45c-4f71-bd27-f45ee377e00a').addClass('show')
            $('#collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a').addClass('show')
            $('#requests-69fd433f-b74e-4dba-833c-75508e4a9918').removeClass('collapsed')
            $('#collapse-69fd433f-b74e-4dba-833c-75508e4a9918').removeClass('collapsed')
            $('#requests-69fd433f-b74e-4dba-833c-75508e4a9918').addClass('show')
            $('#collapse-69fd433f-b74e-4dba-833c-75508e4a9918').addClass('show')
            $('#requests-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').removeClass('collapsed')
            $('#collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').removeClass('collapsed')
            $('#requests-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').addClass('show')
            $('#collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').addClass('show')
        $('#openAllRequests').html("Collapse Requests");
        break;
    case 2:
            $('#requests-dab02eae-1019-44cf-804f-9298ec59065d').addClass('collapsed')
            $('#collapse-dab02eae-1019-44cf-804f-9298ec59065d').addClass('collapsed')
            $('#requests-dab02eae-1019-44cf-804f-9298ec59065d').removeClass('show')
            $('#collapse-dab02eae-1019-44cf-804f-9298ec59065d').removeClass('show')
            $('#requests-81605c50-fe5e-43cd-8fb8-2f64757e07e0').addClass('collapsed')
            $('#collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0').addClass('collapsed')
            $('#requests-81605c50-fe5e-43cd-8fb8-2f64757e07e0').removeClass('show')
            $('#collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0').removeClass('show')
            $('#requests-16d31790-216d-4738-a31f-7d04524ac04c').addClass('collapsed')
            $('#collapse-16d31790-216d-4738-a31f-7d04524ac04c').addClass('collapsed')
            $('#requests-16d31790-216d-4738-a31f-7d04524ac04c').removeClass('show')
            $('#collapse-16d31790-216d-4738-a31f-7d04524ac04c').removeClass('show')
            $('#requests-0be779c4-b45c-4f71-bd27-f45ee377e00a').addClass('collapsed')
            $('#collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a').addClass('collapsed')
            $('#requests-0be779c4-b45c-4f71-bd27-f45ee377e00a').removeClass('show')
            $('#collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a').removeClass('show')
            $('#requests-69fd433f-b74e-4dba-833c-75508e4a9918').addClass('collapsed')
            $('#collapse-69fd433f-b74e-4dba-833c-75508e4a9918').addClass('collapsed')
            $('#requests-69fd433f-b74e-4dba-833c-75508e4a9918').removeClass('show')
            $('#collapse-69fd433f-b74e-4dba-833c-75508e4a9918').removeClass('show')
            $('#requests-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').addClass('collapsed')
            $('#collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').addClass('collapsed')
            $('#requests-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').removeClass('show')
            $('#collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').removeClass('show')
        $('#openAllRequests').html("Expand Requests");
        break;
}
clickCount = clickCount > 1 ? 1 : ++clickCount;
$(this).data("clickCount", clickCount)
})
</script>

<!-- Show/Hide The Skipped Tests -->

<script>
$('#openAllSkipped').on('click', function(e) {
let clickCount = $(this).data("clickCount") || 1
switch (clickCount){
    case 1:
            $('#skipped-dab02eae-1019-44cf-804f-9298ec59065d').removeClass('collapsed')
            $('#skipped-collapse-dab02eae-1019-44cf-804f-9298ec59065d').removeClass('collapsed')
            $('#skipped-dab02eae-1019-44cf-804f-9298ec59065d').addClass('show')
            $('#skipped-collapse-dab02eae-1019-44cf-804f-9298ec59065d').addClass('show')
            $('#skipped-81605c50-fe5e-43cd-8fb8-2f64757e07e0').removeClass('collapsed')
            $('#skipped-collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0').removeClass('collapsed')
            $('#skipped-81605c50-fe5e-43cd-8fb8-2f64757e07e0').addClass('show')
            $('#skipped-collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0').addClass('show')
            $('#skipped-16d31790-216d-4738-a31f-7d04524ac04c').removeClass('collapsed')
            $('#skipped-collapse-16d31790-216d-4738-a31f-7d04524ac04c').removeClass('collapsed')
            $('#skipped-16d31790-216d-4738-a31f-7d04524ac04c').addClass('show')
            $('#skipped-collapse-16d31790-216d-4738-a31f-7d04524ac04c').addClass('show')
            $('#skipped-0be779c4-b45c-4f71-bd27-f45ee377e00a').removeClass('collapsed')
            $('#skipped-collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a').removeClass('collapsed')
            $('#skipped-0be779c4-b45c-4f71-bd27-f45ee377e00a').addClass('show')
            $('#skipped-collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a').addClass('show')
            $('#skipped-69fd433f-b74e-4dba-833c-75508e4a9918').removeClass('collapsed')
            $('#skipped-collapse-69fd433f-b74e-4dba-833c-75508e4a9918').removeClass('collapsed')
            $('#skipped-69fd433f-b74e-4dba-833c-75508e4a9918').addClass('show')
            $('#skipped-collapse-69fd433f-b74e-4dba-833c-75508e4a9918').addClass('show')
            $('#skipped-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').removeClass('collapsed')
            $('#skipped-collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').removeClass('collapsed')
            $('#skipped-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').addClass('show')
            $('#skipped-collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').addClass('show')
        $('#openAllSkipped').html("Collapse All Skipped Tests");
        break;
    case 2:
            $('#skipped-dab02eae-1019-44cf-804f-9298ec59065d').addClass('collapsed')
            $('#skipped-collapse-dab02eae-1019-44cf-804f-9298ec59065d').addClass('collapsed')
            $('#skipped-dab02eae-1019-44cf-804f-9298ec59065d').removeClass('show')
            $('#skipped-collapse-dab02eae-1019-44cf-804f-9298ec59065d').removeClass('show')
            $('#skipped-81605c50-fe5e-43cd-8fb8-2f64757e07e0').addClass('collapsed')
            $('#skipped-collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0').addClass('collapsed')
            $('#skipped-81605c50-fe5e-43cd-8fb8-2f64757e07e0').removeClass('show')
            $('#skipped-collapse-81605c50-fe5e-43cd-8fb8-2f64757e07e0').removeClass('show')
            $('#skipped-16d31790-216d-4738-a31f-7d04524ac04c').addClass('collapsed')
            $('#skipped-collapse-16d31790-216d-4738-a31f-7d04524ac04c').addClass('collapsed')
            $('#skipped-16d31790-216d-4738-a31f-7d04524ac04c').removeClass('show')
            $('#skipped-collapse-16d31790-216d-4738-a31f-7d04524ac04c').removeClass('show')
            $('#skipped-0be779c4-b45c-4f71-bd27-f45ee377e00a').addClass('collapsed')
            $('#skipped-collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a').addClass('collapsed')
            $('#skipped-0be779c4-b45c-4f71-bd27-f45ee377e00a').removeClass('show')
            $('#skipped-collapse-0be779c4-b45c-4f71-bd27-f45ee377e00a').removeClass('show')
            $('#skipped-69fd433f-b74e-4dba-833c-75508e4a9918').addClass('collapsed')
            $('#skipped-collapse-69fd433f-b74e-4dba-833c-75508e4a9918').addClass('collapsed')
            $('#skipped-69fd433f-b74e-4dba-833c-75508e4a9918').removeClass('show')
            $('#skipped-collapse-69fd433f-b74e-4dba-833c-75508e4a9918').removeClass('show')
            $('#skipped-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').addClass('collapsed')
            $('#skipped-collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').addClass('collapsed')
            $('#skipped-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').removeClass('show')
            $('#skipped-collapse-9c48ff67-bb55-427c-a72c-e25a8f8aadcd').removeClass('show')
        $('#openAllSkipped').html("Expand All Skipped Tests");
        break;
}
clickCount = clickCount > 1 ? 1 : ++clickCount;
$(this).data("clickCount", clickCount)
})
</script>

<!-- Show/Hide The Failures -->

<script>
$('#openAllFailed').on('click', function(e) {
let clickCount = $(this).data("clickCount") || 1
let failedItemContent
let failedItemHeading
switch (clickCount){
    case 1:
            failedItemHeading = $('#fails-9bfd556b-3e5a-4cb9-87c3-7a35e3d95f68');
            failedItemContent = $("#fails-collapse-9bfd556b-3e5a-4cb9-87c3-7a35e3d95f68");
            failedItemHeading.removeClass('collapsed')
            failedItemContent.removeClass('collapsed')
            failedItemHeading.addClass('show')
            failedItemContent.addClass('show')
        $('#openAllFailed').html("Collapse All Failed Tests");
        break;
    case 2:
            failedItemHeading = $('#fails-9bfd556b-3e5a-4cb9-87c3-7a35e3d95f68');
            failedItemContent = $("#fails-collapse-9bfd556b-3e5a-4cb9-87c3-7a35e3d95f68");
            failedItemHeading.removeClass('show')
            failedItemContent.removeClass('show')
            failedItemHeading.addClass('collapsed')
            failedItemContent.addClass('collapsed')
        $('#openAllFailed').html("Expand All Failed Tests");
        break;
}
clickCount = clickCount > 1 ? 1 : ++clickCount;
$(this).data("clickCount", clickCount)
})
</script>

<!-- Pretty Print the Response Body-->

<script>
function isJSON(data)
{
    var ret = true;
    try {
            JSON.parse(data);
    }catch(e) {
            ret = false;
    }
    return ret;
}

function isXML(data)
{
    return (data.length > 0 && data[0] === '<');
}

// see https://gist.github.com/sente/1083506/d2834134cd070dbcc08bf42ee27dabb746a1c54d#gistcomment-2254622
function formatXML(data) {
    const PADDING = ' '.repeat(2); // set desired indent size here
    const reg = /(>)(<)(\/*)/g;
    let pad = 0;
    xml = data.replace(reg, '$1\r\n$2$3');

    return xml.split('\r\n').map((node, index) => {
        let indent = 0;
        if (node.match(/.+<\/\w[^>]*>$/)) {
            indent = 0;
        } else if (node.match(/^<\/\w/) && pad > 0) {
            pad -= 1;
        } else if (node.match(/^<\w[^>]*[^\/]>.*$/)) {
            indent = 1;
        } else {
            indent = 0;
        }

        pad += indent;
        return PADDING.repeat(pad - indent) + node;
    }).join('\r\n');
}

$(".prettyPrint").each(function () {
        var data = $(this).text();
        // Verify whether data is JSON
        if(isJSON(data))
        {
                obj = JSON.parse(data);
                data = JSON.stringify(obj, null, 2);
        }
        else if(isXML(data)) {
            data = formatXML(data);
        }
        $(this).text(data);
});
</script>


<!-- Copy Response Body To Clipboard -->

<script>
    var clipboard = new ClipboardJS('.copyButton');

    clipboard.on('success', function(e) {
        e.clearSelection();
        $(".copyButton").addClass("bg-success text-white")
        e.trigger.textContent = '✔ Copied!';
        window.setTimeout(function() {
            $(".copyButton").removeClass("bg-success text-white")
            e.trigger.textContent = 'Copy to Clipboard';
        }, 2000);
    });
    clipboard.on('error', function(e) {
        e.clearSelection();
        $(".copyButton").addClass("bg-danger text-white")
        e.trigger.textContent = '✗ Not Copied';
        window.setTimeout(function() {
            $(".copyButton").removeClass("bg-danger text-white")
            e.trigger.textContent = 'Copy to Clipboard';
        }, 2000);
    });

</script>

<!-- Render the Description Markdown and link in the test failures -->

<script>
    const remarkable = new Remarkable();

    const descriptions = document.querySelectorAll(".renderMarkdown");
    descriptions.forEach(description => {
        description.innerHTML = renderHtmlFromMarkdown(description.textContent);
    });
    function renderHtmlFromMarkdown(markdown) {
        return remarkable.render(trim(markdown));
    }
    function trim(string) {
        return string ? string.replace(/^ +| +$/gm, "") : string;
    }
</script>

<!-- Show/Hide The Toggles When Scrolling The Page -->

<script>
window.onscroll = function() {scrollFunction()};

function scrollFunction() {
  if (document.body.scrollTop > 20 || document.documentElement.scrollTop > 20) {
    document.getElementById("topOfRequestsScreen").style.display = "block";
    document.getElementById("topOfFailuresScreen").style.display = "block";
    document.getElementById("topOfSkippedScreen").style.display = "block";
    document.getElementById("openAll").style.display = "none";
    document.getElementById("openAllRequests").style.display = "none";


    document.getElementById("showOnlyFailures").style.display = "none";
    document.getElementById("openAllFailed").style.display = "none";
  } else {
    document.getElementById("topOfRequestsScreen").style.display = "none";
    document.getElementById("topOfFailuresScreen").style.display = "none";
    document.getElementById("topOfSkippedScreen").style.display = "none";
    document.getElementById("openAll").style.display = "block";
    document.getElementById("openAllRequests").style.display = "block";

    document.getElementById("showOnlyFailures").style.display = "block";
    document.getElementById("openAllFailed").style.display = "block";
  }
}

function topFunction() {
  document.body.scrollTop = 0;
  document.documentElement.scrollTop = 0;
}
</script>

<!-- Creates The Iteration Tabs -->

<script type="text/javascript">
    "use strict";

window.onload = function () {

  // set display for all blocks of response
  var allItems = document.querySelectorAll('[class*=iteration-]');
  allItems.forEach(function(elem){
    elem.style.display = 'block';
  });

   
  let totalIterations = 1;
   

  let menu = document.querySelector('#execution-menu .nav');

  for(var i = 0; i < totalIterations; i++)
  {
    let li = document.createElement('li');
    li.appendChild(document.createTextNode((i + 1)));
    li.setAttribute('id', 'iteration-' + i);
    li.classList.add("custom-tab");
    li.classList.add("itPassed");

    li.addEventListener('click', function() {
      //set display to none for all except row
      let allItems = document.querySelectorAll('[class*=iteration-]:not(.row)');
      allItems.forEach(function(elem) {
        elem.style.display = 'none';
      })

      let allMenus = document.querySelectorAll('[id*=iteration-]');
      allMenus.forEach(function(elem){
        elem.style.borderBottom = 'none';
      })

      this.style.borderBottom = 'solid 3px #032a33';

      let items = document.querySelectorAll("." + this.id + ':not(.row)');
      items.forEach(function(elem) {
        elem.style.display = elem.style.display == 'block' ? 'none' : 'block';
      })
    });
    menu.appendChild(li);
  }

  //shows first tab data
  document.getElementById('iteration-0').click();
  document.getElementById('iterationSelected').innerHTML = `Iteration ${document.getElementById('iteration-0').innerHTML} selected`

    $("#iteration-0").removeClass('itPassed').addClass('itFailed')
}
</script>

<!-- Create the Selected Iteration Label -->

<script>
$(document).ready(function(){
    $(function() {
        $("#iterationList li").click(function() {
            document.getElementById('iterationSelected').innerHTML = "Iteration " + this.innerHTML + " selected"
        });
    });
});
</script>

<!-- Filter Action for the Iterations -->

<script>
$("#filterInput").on("input paste", function() {
    var value = $(this).val();
    $("#iterationList li").filter(function() {
	    $("#showOnlyFailures").data("clickCount") ? $("#showOnlyFailures").click():null;
        $(this).toggle($(this).text().indexOf(value) > -1)
    });
});
</script>

<!-- Showing the Failed Interations -->

<script>
$('#showOnlyFailures').on('click', function(e) {
    let clickCount = $(this).data("clickCount") || 1
	$("#filterInput").val()!="" && clickCount==1 ? $("#filterInput").val('').trigger('input'): null;
    let selectedIteration = $('#iterationList li').filter(function () {
        return $(this).css('border-bottom').indexOf("solid") > -1 && $(this).hasClass('itFailed');
    });
    selectedIteration.length || clickCount > 1 ? null : $("#iterationList li.itFailed")[0].click()
    $("#iterationList li.itPassed").toggle()
    $("div.bg-success [id*=requests]").parents('[class^="row iteration-"]').toggle();
    clickCount = clickCount > 1 ? 1 : ++clickCount;
    clickCount > 1 ? $("#showOnlyFailures").html("Show All Iterations") : $("#showOnlyFailures").html("Show Failed Iterations");
    $(this).data("clickCount", clickCount)
})
</script>
</body>
</html>




 
