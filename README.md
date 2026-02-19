<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Online School Registration</title>

<style>
body{
    font-family: Arial, sans-serif;
    margin:0;
    background: linear-gradient(135deg,#2c3e50,#4ca1af);
    display:flex;
    justify-content:center;
    align-items:center;
    min-height:100vh;
}

.container{
    background:#fff;
    width:95%;
    max-width:500px;
    padding:25px;
    border-radius:15px;
    box-shadow:0 10px 25px rgba(0,0,0,0.3);
}

h2{
    text-align:center;
}

input, select{
    width:100%;
    padding:12px;
    margin:8px 0;
    border-radius:8px;
    border:1px solid #ccc;
}

button{
    width:100%;
    padding:12px;
    border:none;
    border-radius:8px;
    background:#3498db;
    color:white;
    font-size:16px;
    cursor:pointer;
    margin-top:10px;
}

button:hover{
    background:#2980b9;
}

.student-card{
    background:#f4f4f4;
    padding:10px;
    border-radius:8px;
    margin-top:10px;
}

.delete-btn{
    background:red;
    margin-top:5px;
}

.hidden{
    display:none;
}
</style>
</head>

<body>

<div class="container">

<!-- Registration Form -->
<div id="formPage">
<h2>Online School Registration</h2>

<form id="studentForm">
<input type="text" id="firstName" placeholder="First Name" required>
<input type="text" id="fatherName" placeholder="Father Name" required>
<input type="number" id="age" placeholder="Age" required>
<input type="email" id="email" placeholder="Email Address" required>

<select id="grade" required>
<option value="">Select Grade</option>
<option>Grade 9</option>
<option>Grade 10</option>
<option>Grade 11</option>
<option>Grade 12</option>
</select>

<button type="submit">Register</button>
</form>

<button onclick="showStudents()">View Registered Students</button>
</div>

<!-- Students List Page -->
<div id="studentsPage" class="hidden">
<h2>Registered Students</h2>
<div id="studentsList"></div>
<button onclick="goBack()">Back to Registration</button>
</div>

</div>

<script>
function getStudents(){
    return JSON.parse(localStorage.getItem("students")) || [];
}

function saveStudents(students){
    localStorage.setItem("students", JSON.stringify(students));
}

document.getElementById("studentForm").addEventListener("submit", function(e){
    e.preventDefault();

    const students = getStudents();

    const newStudent = {
        firstName: document.getElementById("firstName").value,
        fatherName: document.getElementById("fatherName").value,
        age: document.getElementById("age").value,
        email: document.getElementById("email").value,
        grade: document.getElementById("grade").value
    };

    students.push(newStudent);
    saveStudents(students);

    alert("Registration Successful!");

    document.getElementById("studentForm").reset();
});

function showStudents(){
    document.getElementById("formPage").classList.add("hidden");
    document.getElementById("studentsPage").classList.remove("hidden");

    const students = getStudents();
    const list = document.getElementById("studentsList");
    list.innerHTML="";

    students.forEach((student,index)=>{
        list.innerHTML += `
        <div class="student-card">
        <strong>${student.firstName} ${student.fatherName}</strong><br>
        Age: ${student.age}<br>
        Email: ${student.email}<br>
        Grade: ${student.grade}<br>
        <button class="delete-btn" onclick="deleteStudent(${index})">Delete</button>
        </div>
        `;
    });
}

function deleteStudent(index){
    const students = getStudents();
    students.splice(index,1);
    saveStudents(students);
    showStudents();
}

function goBack(){
    document.getElementById("studentsPage").classList.add("hidden");
    document.getElementById("formPage").classList.remove("hidden");
}
</script>

</body>
</html>
