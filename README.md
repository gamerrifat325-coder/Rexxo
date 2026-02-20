<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RexxoTube</title>

<style>
body{
  margin:0;
  font-family:Arial;
  background:#0f0f0f;
  color:white;
}

header{
  background:black;
  padding:15px;
  font-size:22px;
  font-weight:bold;
  position:sticky;
  top:0;
  border-bottom:1px solid #222;
}

.container{
  padding:10px;
  margin-bottom:70px;
}

.video-card{
  background:#181818;
  padding:10px;
  border-radius:8px;
  margin-bottom:15px;
}

video{
  width:100%;
  border-radius:8px;
}

button{
  padding:8px 12px;
  border:none;
  border-radius:5px;
  margin-top:5px;
  margin-right:5px;
  font-weight:bold;
}

.like{background:#2563eb;color:white;}
.delete{background:red;color:white;}

.bottom-nav{
  position:fixed;
  bottom:0;
  width:100%;
  background:black;
  display:flex;
  justify-content:space-around;
  padding:10px 0;
  border-top:1px solid #222;
}

.bottom-nav button{
  background:none;
  color:white;
}

.hidden{display:none;}
input{
  padding:10px;
  width:95%;
  margin-bottom:10px;
  border-radius:5px;
  border:none;
}
.upload-btn{
  background:#22c55e;
  color:white;
  width:100%;
}
</style>
</head>
<body>

<header>🔥 RexxoTube</header>

<div class="container">

<!-- HOME PAGE -->
<div id="homePage">

  <div id="videoList"></div>

</div>

<!-- SHORTS PAGE -->
<div id="shortsPage" class="hidden">
  <h3>🎬 Shorts</h3>
  <p>Shorts feature coming soon...</p>
</div>

<!-- UPLOAD PAGE -->
<div id="uploadPage" class="hidden">
  <h3>📤 Upload Video</h3>
  <input type="text" id="videoTitle" placeholder="Enter Video Title">
  <input type="file" id="videoFile" accept="video/*">
  <button class="upload-btn" onclick="uploadVideo()">Upload</button>
</div>

</div>

<!-- BOTTOM NAV -->
<div class="bottom-nav">
  <button onclick="showHome()">🏠 Home</button>
  <button onclick="showShorts()">🎬 Shorts</button>
  <button onclick="showUpload()">➕ Upload</button>
</div>

<script>

let videos = JSON.parse(localStorage.getItem("videos")) || [];

function saveData(){
  localStorage.setItem("videos", JSON.stringify(videos));
}

function renderVideos(){
  let list = document.getElementById("videoList");
  list.innerHTML="";

  if(videos.length===0){
    list.innerHTML="<p>No video uploaded yet.</p>";
    return;
  }

  videos.forEach((video,index)=>{
    list.innerHTML+=`
      <div class="video-card">
        <h4>${video.title}</h4>
        <video controls src="${video.url}"></video>
        <br>
        <button class="like" onclick="likeVideo(${index})">👍 ${video.likes}</button>
        <button class="delete" onclick="deleteVideo(${index})">🗑 Delete</button>
      </div>
    `;
  });
}

function uploadVideo(){
  let title=document.getElementById("videoTitle").value;
  let file=document.getElementById("videoFile").files[0];

  if(!title || !file){
    alert("Enter title and select video");
    return;
  }

  let url=URL.createObjectURL(file);

  videos.push({
    title:title,
    url:url,
    likes:0
  });

  saveData();
  renderVideos();
  showHome();

  document.getElementById("videoTitle").value="";
  document.getElementById("videoFile").value="";
}

function likeVideo(index){
  videos[index].likes++;
  saveData();
  renderVideos();
}

function deleteVideo(index){
  videos.splice(index,1);
  saveData();
  renderVideos();
}

function showHome(){
  document.getElementById("homePage").classList.remove("hidden");
  document.getElementById("shortsPage").classList.add("hidden");
  document.getElementById("uploadPage").classList.add("hidden");
}

function showShorts(){
  document.getElementById("homePage").classList.add("hidden");
  document.getElementById("shortsPage").classList.remove("hidden");
  document.getElementById("uploadPage").classList.add("hidden");
}

function showUpload(){
  document.getElementById("homePage").classList.add("hidden");
  document.getElementById("shortsPage").classList.add("hidden");
  document.getElementById("uploadPage").classList.remove("hidden");
}

renderVideos();

</script>

</body>
</html>
