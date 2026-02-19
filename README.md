# STUDENT-ENROLLMENT
 index.html
 style.css
 script.js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>STUDENT ENROLLMENT</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: navy;
      color: white;
      margin: 0;
      padding: 0;
    }
    h1 {
      text-align: center;
      padding: 20px;
      background: #003366;
      margin: 0;
    }
    .container {
      width: 400px;
      margin: 30px auto;
      background: #1a1a40;
      padding: 20px;
      border-radius: 10px;
    }
    h2 {
      text-align: center;
    }
    input, select {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border-radius: 5px;
      border: none;
    }
    button {
      width: 100%;
      padding: 10px;
      background: #007acc;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .dashboard {
      display: none;
      margin: 20px auto;
      width: 600px;
      background: #1a1a40;
      padding: 20px;
      border-radius: 10px;
    }
    .photo-box {
      float: right;
      width: 120px;
      height: 120px;
      background: #333;
      border: 2px solid white;
      margin-left: 20px;
      text-align: center;
      line-height: 120px;
      color: #aaa;
    }
    .stats {
      margin-top: 20px;
      background: #333;
      padding: 10px;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <h1>STUDENT ENROLLMENT</h1>

  <div class="container" id="loginBox">
    <h2>Login</h2>
    <input type="text" id="loginUser" placeholder="Username">
    <input type="password" id="loginPass" placeholder="Password">
    <button onclick="login()">Log In</button>
    <p style="text-align:center;">or</p>
    <button onclick="showSignup()">Sign Up / Create Account</button>
  </div>

  <div class="container" id="signupBox" style="display:none;">
    <h2>Create Account</h2>
    <input type="text" id="firstName" placeholder="First Name">
    <input type="text" id="lastName" placeholder="Last Name">
    <input type="text" id="lrn" placeholder="ID LRN">
    <input type="text" id="phone" placeholder="Phone Number">
    <input type="text" id="course" placeholder="Course">
    <input type="date" id="birthdate">
    <select id="gender">
      <option value="">Select Gender</option>
      <option value="Male">Male</option>
      <option value="Female">Female</option>
    </select>
    <input type="text" id="username" placeholder="Username">
    <input type="password" id="password" placeholder="Password">
    <button onclick="signup()">Create Account</button>
  </div>

  <div class="dashboard" id="dashboard">
    <h2>Student Dashboard</h2>
    <div class="photo-box" id="photoBox">Photo</div>
    <input type="file" id="photoUpload" accept="image/*" onchange="previewPhoto()">
    
    <p><b>ID:</b> <span id="studentId"></span></p>
    <p><b>First Name:</b> <span id="studentFirst"></span></p>
    <p><b>Last Name:</b> <span id="studentLast"></span></p>
    <p><b>ID LRN:</b> <span id="studentLRN"></span></p>
    <p><b>Phone:</b> <span id="studentPhone"></span></p>
    <p><b>Course:</b> <span id="studentCourse"></span></p>
    <p><b>Birthdate:</b> <span id="studentBirth"></span></p>
    <p><b>Gender:</b> <span id="studentGender"></span></p>

    <div class="stats">
      <p>Total Users: <span id="totalUsers">0</span></p>
      <p>Male: <span id="malePercent">0%</span></p>
      <p>Female: <span id="femalePercent">0%</span></p>
    </div>
  </div>

  <script>
    let users = [];
    let loggedInUser = null;

    function showSignup() {
      document.getElementById("loginBox").style.display = "none";
      document.getElementById("signupBox").style.display = "block";
    }

    function signup() {
      const firstName = document.getElementById("firstName").value.trim();
      const lastName = document.getElementById("lastName").value.trim();
      const lrn = document.getElementById("lrn").value.trim();
      const phone = document.getElementById("phone").value.trim();
      const course = document.getElementById("course").value.trim();
      const birthdate = document.getElementById("birthdate").value;
      const gender = document.getElementById("gender").value;
      const username = document.getElementById("username").value.trim();
      const password = document.getElementById("password").value.trim();

      if (!firstName || !lastName || !lrn || !phone || !course || !birthdate || !gender || !username || !password) {
        alert("Sign up Failed: All fields are required!");
        return;
      }

      const user = {
        id: users.length + 1,
        firstName, lastName, lrn, phone, course, birthdate, gender, username, password
      };
      users.push(user);
      alert("Account created successfully!");
      document.getElementById("signupBox").style.display = "none";
      document.getElementById("loginBox").style.display = "block";
    }

    function login() {
      const username = document.getElementById("loginUser").value;
      const password = document.getElementById("loginPass").value;
      const user = users.find(u => u.username === username && u.password === password);
      if (user) {
        loggedInUser = user;
        showDashboard();
      } else {
        alert("Invalid login!");
      }
    }

    function showDashboard() {
      document.getElementById("loginBox").style.display = "none";
      document.getElementById("signupBox").style.display = "none";
      document.getElementById("dashboard").style.display = "block";

      document.getElementById("studentId").innerText = loggedInUser.id;
      document.getElementById("studentFirst").innerText = loggedInUser.firstName;
      document.getElementById("studentLast").innerText = loggedInUser.lastName;
      document.getElementById("studentLRN").innerText = loggedInUser.lrn;
      document.getElementById("studentPhone").innerText = loggedInUser.phone;
      document.getElementById("studentCourse").innerText = loggedInUser.course;
      document.getElementById("studentBirth").innerText = loggedInUser.birthdate;
      document.getElementById("studentGender").innerText = loggedInUser.gender;

      updateStats();
    }

    function updateStats() {
      const total = users.length;
      const males = users.filter(u => u.gender === "Male").length;
      const females = users.filter(u => u.gender === "Female").length;

      document.getElementById("totalUsers").innerText = total;
      document.getElementById("malePercent").innerText = ((males / total) * 100).toFixed(1) + "%";
      document.getElementById("femalePercent").innerText = ((females / total) * 100).toFixed(1) + "%";
    }

    function previewPhoto() {
      const file = document.getElementById("photoUpload").files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
          document.getElementById("photoBox").style.backgroundImage = `url(${e.target.result})`;
          document.getElementById("photoBox").style.backgroundSize = "cover";
          document.getElementById("photoBox").innerText = "";
        };
        reader.readAsDataURL(file);
      }
    }
  </script>
</body>
</html>
