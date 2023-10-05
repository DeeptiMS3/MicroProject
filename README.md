// MicroProject
//A MicroProject on JASONPOWERDB


<!DOCTYPE html>
<html lang="en"> 

<head>
<title>Student Enrollment Form</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
</head>

<body>

<div class="container">
	<div class="page-header text-center"><h2>Student Enrollment Form</h2></div>

<form id="empForm" method="get">

<div class="form-group">
<label >Roll no:</label> <input type="text" class="form-control" id="rollno" placeholder="Enter Roll no" onchange="getStd()"required></div>

<div class="form-group">
<label >Full-Name:</label> <input type="text" class="form-control" id="name" placeholder="Enter Full-Name" required></div>

<div class="form-group">
<label >Class:</label> <input type="text" class="form-control" id="Class" placeholder="Enter Class" required></div>

<div class="form-group">
<label >Birth-Date:</label> <input type="date" class="form-control" id="bd" placeholder="Enter Birth-Date" required></div>

<div class="form-group">
<label >Address:</label> <input type="text" class="form-control" id="add" placeholder="Enter Address" required></div>

<div class="form-group">
<label >Enrollment-Date:</label> <input type="date" class="form-control" id="ed" placeholder="Enter Enrollment-Date" required></div>

<div class="form-group text-center">
<button type="button" class="btn btn-lg btn-primary" id="save" onclick="saveData()" disabled>Save</button>
<button type="button" class="btn btn-lg btn-primary" id="change" onclick="changeData()" disabled>Update</button>
<button type="button" class="btn btn-lg btn-primary" id="reset" onclick="resetForm()" disabled>Reset</button>
</div>
</form>
</div>

<script src="https://login2explore.com/jpdb/resources/js/0.0.3/jpdb-commons.js"></script>

<script type="text/javascript">

var BaseURL="http://api.login2explore.com:5577";
var IRL="/api/irl";
var IML="/api/iml";
var DBName=" STUDENT-DB";
var REL=" STUDENT-TABLE";
var connToken="90931632|-31949326629299227|90961883";

$('#rollno').focus();

function saveRecNO(jsonObj){
	var lvData=JSON.parse(jsonObj.data);
	localStorage.setItem('recno',lvData.rec_no);
}

function getStdIDAsJsonObj(){
	var rollno =$('#rollno').val();
	var jsonStr={
		rollno: rollno
	};
	return JSON.stringify(jsonStr);
}

function fillData(jsonObj){
	saveRecNO(jsonObj);
	var record=JSON.parse(jsonObj.data).record;
	
	$('#name').val(record.name);
	$('#Class').val(record.class);
	$('#bd').val(record.birthdate);
	$('#add').val(record.address);
	$('#ed').val(record.enrollmentdate);
}

function resetForm(){
	$('#rollno').val("");
	$('#name').val("");
	$('#Class').val("");
	$('#bd').val("");
	$('#add').val("");
	$('#ed').val("");
	$('#rollno').prop('disabled',false);
	$('#save').prop('disabled',true);
	$('#change').prop('disabled',true);
	$('#reset').prop('disabled',true);
	$('#rollno').focus();
}

function validateData() {
    var rollno, name, Class, bd, add, ed;
    rollno = $('#rollno').val();
    name = $('#name').val();
    Class = $('#Class').val();
    bd = $('#bd').val();
    add = $('#add').val();
    ed = $('#ed').val();

    if (rollno === "") {
        alert("Rollno Missing");
        $('#rollno').focus();
        return '';
    }
    if (name === "") {
        alert("Name Missing");
        $('#name').focus();
        return '';
    }
    if (Class === "") {
        alert("Class Missing");
        $('#Class').focus();
        return '';
    }
    if (bd === "") {
        alert("Birth-date Missing");
        $('#bd').focus();
        return '';
    }
    if (add === "") {
        alert("Address Missing");
        $('#add').focus();
        return '';
    }
    if (ed === "") {
        alert("Enrollment-Date Missing");
        $('#ed').focus();
        return '';
    }

    var jsonStrObj = {
        rollno: rollno,
        name: name,
        class: Class,
        birthdate: bd,
        address: add,
        enrollmentdate: ed
    };

    return JSON.stringify(jsonStrObj);
}


function getStd(){
	var stdIdJsonObj=getStdIDAsJsonObj();
	var getRequest=createGET_BY_KEYRequest(connToken, DBName, REL, stdIdJsonObj);
	jQuery.ajaxSetup({async: false});
	var resJsonObj=executeCommandAtGivenBaseUrl(getRequest, BaseURL, IRL);
	jQuery.ajaxSetup({async: true});
	if(resJsonObj.status === 400 || resJsonObj.status === 404){
		$('#save').prop('disabled',false);
		$('#reset').prop('disabled',false);
		$('#name').focus();
	}
	else if(resJsonObj.status === 200){
		$('#rollno').prop('disabled',true);
		fillData(resJsonObj);
		$('#change').prop('disabled',false);
		$('#reset').prop('disabled',false);
		$('#name').focus();

	}
}

function saveData(){
	var jsonStrObj=validateData();
	if(jsonStrObj === ''){
		return '';
	}
	var putRequest=createPUTRequest(connToken, jsonStrObj, DBName, REL);
	jQuery.ajaxSetup({async: false});
	var resJsonObj=executeCommandAtGivenBaseUrl(putRequest, BaseURL, IML);
	jQuery.ajaxSetup({async: true});
	resetForm();
	$('#rollno').focus();
}

function changeData(){
	$('#change').prop('disabled',true);
	jsonChg=validateData();
	var updateRequest= createUPDATERecordRequest(connToken, jsonChg, DBName, REL, localStorage.getItem('recno'));
	jQuery.ajaxSetup({async: false});
	var resJsonObj=executeCommandAtGivenBaseUrl(updateRequest, BaseURL, IML);
	jQuery.ajaxSetup({async: true});
	console.log(resJsonObj);
	resetForm();
	$('#rollno').focus();
}

</script>

</body>
</html>
