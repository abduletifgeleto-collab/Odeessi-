<!DOCTYPE html>
<html lang="om">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Odeessi - Appii Hawaasummaa Guutuu</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <meta name="description" content="Odeessi - Appii hawaasummaa Afaan Oromoo kan ergaa walii erguufi suuraa maxxansuuf gargaaru.">
    <meta name="keywords" content="Odeessi, Afaan Oromo Social Media, Ethiopian App">
    
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-database-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-auth-compat.js"></script>

    <style>
        :root {
            --primary: #1877f2;
            --bg-body: #f0f2f5;
            --bg-card: #ffffff;
            --text-main: #1c1e21;
            --sidebar-width: 250px;
            --border-color: #ddd;
        }

        .dark-mode {
            --bg-body: #18191a;
            --bg-card: #242526;
            --text-main: #e4e6eb;
            --border-color: #3e4042;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            margin: 0;
            transition: 0.3s;
        }

        .page { display: none; min-height: 100vh; width: 100%; }
        .page.active { display: flex; }

        #login-page {
            justify-content: center;
            align-items: center;
            background: linear-gradient(135deg, #1877f2, #00c6ff);
        }
        .login-card {
            background: white;
            padding: 2.5rem;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.2);
            text-align: center;
            width: 380px;
        }
        .input-group input {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 8px;
            box-sizing: border-box;
        }

        .sidebar {
            width: var(--sidebar-width);
            background: var(--bg-card);
            height: 100vh;
            position: fixed;
            border-right: 1px solid var(--border-color);
            padding: 20px;
            z-index: 100;
        }
        .nav-item {
            padding: 12px 15px;
            margin: 5px 0;
            cursor: pointer;
            border-radius: 10px;
            display: flex;
            align-items: center; gap: 12px;
            transition: 0.2s;
        }
        .nav-item:hover, .nav-item.active {
            background: rgba(24, 119, 242, 0.1);
            color: var(--primary);
            font-weight: bold;
        }

        .main-content {
            margin-left: var(--sidebar-width);
            width: calc(100% - var(--sidebar-width));
            padding: 25px;
            box-sizing: border-box;
        }

        .top-nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: var(--bg-card);
            padding: 12px 25px;
            margin-bottom: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        .card {
            background: var(--bg-card);
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 20px;
            border: 1px solid var(--border-color);
            box-shadow: 0 1px 2px rgba(0,0,0,0.1);
        }
        .view { display: none; width: 100%; max-width: 700px; margin: 0 auto; }
        .view.active { display: block; }

        .user-circle { width: 45px; height: 45px; background: #ccc; border-radius: 50%; overflow: hidden; }
        .user-circle img { width: 100%; height: 100%; object-fit: cover; }
        
        .stories { display: flex; gap: 10px; margin-bottom: 20px; overflow-x: auto; padding-bottom: 5px; }
        .story-box { min-width: 100px; height: 160px; background: #ddd; border-radius: 10px; cursor: pointer; background-size: cover; }
        .story-box.add { background: #e4e6eb; display: flex; align-items: center; justify-content: center; font-size: 2rem; color: #65676b; }

        .msg-bubble { background: #e4e6eb; padding: 10px 15px; border-radius: 18px; margin-bottom: 8px; max-width: 75%; width: fit-content; color: #000; }
        .msg-own { background: var(--primary); color: white; align-self: flex-end; }
        .voice-msg { display: flex; align-items: center; gap: 10px; background: #f0f2f5; padding: 10px; border-radius: 20px; width: 200px; color: #333; margin: 5px 0; border: 1px solid #ddd; }
        
        .notif-item { display: flex; align-items: center; gap: 15px; padding: 12px; border-bottom: 1px solid var(--border-color); }
        .notif-icon { width: 35px; height: 35px; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; }

        .btn-main { background: var(--primary); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; font-weight: bold; width: 100%; }
        #imgPreview { width: 100%; border-radius: 10px; margin-top: 15px; display: none; max-height: 350px; object-fit: cover; }

        .settings-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 15px; }
        .s-field label { font-size: 0.8rem; display: block; margin-bottom: 5px; font-weight: bold; }
        .s-field input, .s-field select { width: 100%; padding: 10px; border-radius: 6px; border: 1px solid var(--border-color); background: var(--bg-body); color: var(--text-main); }

        #profile-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); z-index: 2000; justify-content: center; align-items: center; }
        .modal-content { background: var(--bg-card); padding: 30px; border-radius: 15px; width: 90%; max-width: 400px; text-align: center; }

        @media (max-width: 768px) {
            .sidebar { width: 70px; padding: 15px 10px; }
            .sidebar span, .sidebar-header { display: none; }
            .main-content { margin-left: 70px; width: calc(100% - 70px); }
            .settings-grid { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body class="light-mode">

    <div id="login-page" class="page active">
        <div class="login-card">
            <h1 style="color:var(--primary); margin:0;">Odeessi</h1>
            <p>Itti fufiinsa jireenya hawaasummaa keetii</p>
            <div class="input-group">
                <input type="email" id="email_kee" placeholder="Email kee">
                <input type="password" id="password" placeholder="Jecha icciitii">
                <button onclick="handleLogin()" class="btn-main">Seeni / Gali</button>
            </div>
        </div>
    </div>

    <div id="main-app" class="page">
        <nav class="sidebar">
            <div class="sidebar-header" style="font-size: 1.5rem; font-weight: bold; color: var(--primary); margin-bottom: 25px;">Odeessi</div>
            <div class="nav-links">
                <div class="nav-item active" onclick="showView('home', this)"><i class="fas fa-home"></i> <span>SEENSA</span></div>
                <div class="nav-item" onclick="showView('messages', this)"><i class="fas fa-comment"></i> <span>ERGAAWWAN</span></div>
                <div class="nav-item" onclick="showView('calls', this)"><i class="fas fa-phone"></i> <span>BILBILA</span></div>
                <div class="nav-item" onclick="showView('notifications', this)"><i class="fas fa-bell"></i> <span>TAATEEWWAN</span></div>
                <div class="nav-item" onclick="showView('users', this)"><i class="fas fa-user-friends"></i> <span>NAMOOTA</span></div>
                <div class="nav-item" onclick="showView('settings', this)"><i class="fas fa-cog"></i> <span>SIRREESSA</span></div>
            </div>
            <div onclick="logout()" style="margin-top: 50px; cursor: pointer; color: #fa3e3e; padding-left: 15px; font-weight: bold;">
                <i class="fas fa-sign-out-alt"></i> <span>Bahi</span>
            </div>
        </nav>

        <main class="main-content">
            <header class="top-nav">
                <div style="font-weight: bold; font-size: 1.2rem;">Odeessi</div>
                <div style="display:flex; align-items:center; gap:20px;">
                    <button onclick="toggleDarkMode()" style="border:none; cursor:pointer; background:none; font-size:1.2rem; color: inherit;"><i class="fas fa-adjust"></i></button>
                    <div class="user-circle"><img id="profile-pic-top" src="https://via.placeholder.com/45"></div>
                </div>
            </header>

            <section id="home" class="view active">
                <div class="stories">
                    <div class="story-box add">+</div>
                    <div class="story-box" style="background-image:url('https://picsum.photos/200/300?1');"></div>
                    <div class="story-box" style="background-image:url('https://picsum.photos/200/300?2');"></div>
                    <div class="story-box" style="background-image:url('https://picsum.photos/200/300?3');"></div>
                </div>

                <div class="card">
                    <div style="display:flex; gap:12px;">
                        <div class="user-circle"><img class="current-user-img" src="https://via.placeholder.com/45"></div>
                        <textarea id="postText" placeholder="Maal yaadaa jirtu?" style="width:100%; border:none; outline:none; background:transparent; color:inherit; font-size:1.1rem; resize:none;"></textarea>
                    </div>
                    <img id="imgPreview" src="">
                    <hr style="opacity:0.1; margin: 15px 0;">
                    <div style="display:flex; justify-content:space-between; align-items:center;">
                        <button onclick="document.getElementById('postImg').click()" style="border:none; background:none; cursor:pointer; color:inherit;"><i class="fas fa-image" style="color:#45bd62"></i> Suuraa</button>
                        <input type="file" id="postImg" hidden accept="image/*" onchange="previewFile()">
                        <button onclick="addNewPost()" class="btn-main" style="width:auto; padding:8px 25px;">Maxxansi</button>
                    </div>
                </div>
                <div id="feeds"></div>
            </section>

            <section id="messages" class="view">
                <div class="card" style="display:flex; height:450px; padding:0; overflow:hidden;">
                    <div style="width:35%; border-right:1px solid var(--border-color); overflow-y:auto;">
                        <div style="padding:15px; background:rgba(24, 119, 242, 0.1); border-bottom:1px solid var(--border-color);">
                            <b>Lammaa Magarsaa</b>
                            <p style="font-size:0.8rem; margin:0; opacity:0.7;">Akkam jirta?</p>
                        </div>
                    </div>
                    <div style="width:65%; display:flex; flex-direction:column; background:var(--bg-body);">
                        <div id="chatBox" style="flex:1; padding:20px; overflow-y:auto; display:flex; flex-direction:column;">
                            <div class="msg-bubble">Akkam jirta obbolee?</div>
                            <div class="voice-msg"><i class="fas fa-play"></i> <span>0:15 Voice Message</span></div>
                            <div class="msg-bubble msg-own">Fayyadha, galatoomi!</div>
                        </div>
                        <div style="padding:15px; background:var(--bg-card); display:flex; gap:10px;">
                            <input type="text" id="chatInput" placeholder="Ergaa barreeffamaa..." style="flex:1; padding:10px; border-radius:20px; border:1px solid var(--border-color); background:var(--bg-body); color:inherit;">
                            <button onclick="alert('Ergameera!')" style="border:none; background:var(--primary); color:white; border-radius:50%; width:40px; height:40px; cursor:pointer;"><i class="fas fa-paper-plane"></i></button>
                        </div>
                    </div>
                </div>
            </section>

            <section id="calls" class="view">
                <h2>Bilbila</h2>
                <div class="card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div style="display:flex; align-items:center; gap:15px;">
                        <div class="user-circle"><img src="https://i.pravatar.cc/150?u=8"></div>
                        <b>Caalaa Bultum</b>
                    </div>
                    <div style="display:flex; gap:10px;">
                        <button onclick="alert('Voice calling...')" style="padding:10px; border-radius:50%; border:none; cursor:pointer;"><i class="fas fa-phone" style="color:#28a745"></i></button>
                        <button onclick="alert('Video calling...')" style="padding:10px; border-radius:50%; border:none; cursor:pointer;"><i class="fas fa-video" style="color:#1877f2"></i></button>
                    </div>
                </div>
            </section>

            <section id="notifications" class="view">
                <h2>Beeksisa</h2>
                <div class="card" style="padding:0;">
                    <div class="notif-item">
                        <div class="notif-icon" style="background:#1877f2;"><i class="fas fa-thumbs-up"></i></div>
                        <p><b>Abdii</b> maxxansa kee jaalateera</p>
                    </div>
                    <div class="notif-item">
                        <div class="notif-icon" style="background:#42b72a;"><i class="fas fa-share"></i></div>
                        <p><b>Soliiana</b> maxxansa kee hirteetti</p>
                    </div>
                    <div class="notif-item">
                        <div class="notif-icon" style="background:#f02849;"><i class="fas fa-comment"></i></div>
                        <p><b>Boonaa</b> yaada (comment) kenneera</p>
                    </div>
                </div>
            </section>

            <section id="users" class="view">
                <h2>Namoota</h2>
                <div class="card" id="user-list-container"></div>
            </section>

            <section id="settings" class="view">
                <h2>Sirreessa</h2>
                <div class="card">
                    <h3><i class="fas fa-user-circle"></i> Profile & Akkaawuntii</h3>
                    <div class="settings-grid">
                        <div class="s-field"><label>Suuraa Profile</label><input type="file" onchange="updateProfilePic(event)"></div>
                        <div class="s-field"><label>Maqaa Guutuu</label><input type="text" id="set_name" placeholder="Jijjiiri..."></div>
                        <div class="s-field"><label>Bilbila</label><input type="tel" placeholder="+251 9..."></div>
                        <div class="s-field"><label>Barnoota</label>
                            <select><option>Digrii</option><option>Dippiloomaa</option><option>Masters</option></select>
                        </div>
                        <div class="s-field"><label>Afaan</label>
                            <select><option>Afaan Oromoo</option><option>English</option></select>
                        </div>
                    </div>
                    <button class="btn-main" style="margin-top:20px; width:auto; padding:10px 30px;" onclick="saveSettings()">Mirkaneessi</button>
                </div>
            </section>
        </main>
    </div>

    <div id="profile-modal" onclick="this.style.display='none'">
        <div class="modal-content" onclick="event.stopPropagation()">
            <img id="m-img" src="" style="width:100px; height:100px; border-radius:50%; margin-bottom:15px; border:3px solid var(--primary);">
            <h2 id="m-name" style="margin:5px;"></h2>
            <p><b>Bakka:</b> <span id="m-place"></span></p>
            <p><b>Barnoota:</b> <span id="m-edu"></span></p>
            <br>
            <button class="btn-main" onclick="document.getElementById('profile-modal').style.display='none'">Cufi</button>
        </div>
    </div>

    <script>
        const firebaseConfig = {
            apiKey: "AIzaSyAVYWlGnrx_2Ay1O1xz5-2ImV1YgkLnq4I",
            authDomain: "odeessi-app.firebaseapp.com",
            databaseURL: "https://odeessi-app-default-rtdb.firebaseio.com", 
            projectId: "odeessi-app",
            storageBucket: "odeessi-app.firebasestorage.app",
            messagingSenderId: "256756112478",
            appId: "1:256756112478:web:54c88681d3f4c75a4f68cf",
            measurementId: "G-6EGGLM390M"
        };
        
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();
        const auth = firebase.auth(); // Amma sirreeffameera

        let currentUser = JSON.parse(localStorage.getItem('user')) || null;
        let postImgBase64 = "";

        const systemUsers = [
            { id: 1, name: "Obbo Alamaayyoo", place: "Adama", edu: "Degree", img: "https://i.pravatar.cc/150?u=1" },
            { id: 2, name: "Faantuu Magarsaa", place: "Jimma", edu: "Masters", img: "https://i.pravatar.cc/150?u=2" },
            { id: 3, name: "Kumaa Dibaabaa", place: "Ambo", edu: "Diploma", img: "https://i.pravatar.cc/150?u=3" }
        ];

        window.onload = () => {
            if(currentUser) loadApp();
            if(localStorage.getItem('dark-mode') === 'enabled') document.body.classList.add('dark-mode');
            renderUsers();
            
            // Real-time Database feed
            database.ref('posts').on('value', (snapshot) => {
                const onlineData = snapshot.val();
                let onlinePosts = onlineData ? Object.values(onlineData).reverse() : [];
                renderPosts(onlinePosts);
            });
        };

        function handleLogin() {
            const email = document.getElementById('email_kee').value;
            const password = document.getElementById('password').value;

            if(!email || !password) {
                alert("Maaloo Email fi Password galchi!");
                return;
            }

            auth.signInWithEmailAndPassword(email, password)
                .then((userCredential) => {
                    currentUser = { 
                        name: userCredential.user.displayName || email.split('@')[0], 
                        avatar: "https://ui-avatars.com/api/?name=" + email 
                    };
                    localStorage.setItem('user', JSON.stringify(currentUser));
                    loadApp();
                })
                .catch((error) => {
                    alert("Dogoggora: " + error.message);
                });
        }

        function loadApp() {
            document.getElementById('login-page').classList.remove('active');
            document.getElementById('main-app').classList.add('active');
            document.getElementById('profile-pic-top').src = currentUser.avatar;
            document.querySelectorAll('.current-user-img').forEach(img => img.src = currentUser.avatar);
        }

        function logout() { localStorage.removeItem('user'); location.reload(); }

        function toggleDarkMode() {
            document.body.classList.toggle('dark-mode');
            localStorage.setItem('dark-mode', document.body.classList.contains('dark-mode') ? 'enabled' : 'disabled');
        }

        function showView(viewId, element) {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(viewId).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            element.classList.add('active');
        }

        function previewFile() {
            const file = document.getElementById('postImg').files[0];
            const reader = new FileReader();
            reader.onloadend = () => {
                document.getElementById('imgPreview').src = reader.result;
                document.getElementById('imgPreview').style.display = "block";
                postImgBase64 = reader.result;
            };
            if (file) reader.readAsDataURL(file);
        }

        function addNewPost() {
            const text = document.getElementById('postText').value;
            if(!text && !postImgBase64) return;
            
            const newPost = { 
                author: currentUser.name, 
                avatar: currentUser.avatar, 
                content: text, 
                image: postImgBase64, 
                date: new Date().toLocaleString() 
            };
            
            // Amma hundi gara Realtime Database ('database') galla
            database.ref('posts').push(newPost)
            .then(() => {
                document.getElementById('postText').value = "";
                document.getElementById('imgPreview').style.display = "none";
                postImgBase64 = "";
            })
            .catch((error) => {
                alert("Dogoggora: " + error.message);
            });
        }

        function renderPosts(postsToRender) {
            const feeds = document.getElementById('feeds');
            feeds.innerHTML = postsToRender.map(p => `
                <div class="card">
                    <div style="display:flex; align-items:center; gap:10px; margin-bottom:12px;">
                        <div class="user-circle"><img src="${p.avatar}"></div>
                        <div><b>${p.author}</b><div style="font-size:0.7rem; opacity:0.5;">${p.date}</div></div>
                    </div>
                    <p>${p.content}</p>
                    ${p.image ? `<img src="${p.image}" style="width:100%; border-radius:10px; margin-top:10px;">` : ''}
                </div>
            `).join('');
        }

        function renderUsers() {
            const container = document.getElementById('user-list-container');
            container.innerHTML = systemUsers.map(u => `
                <div style="display:flex; justify-content:space-between; align-items:center; padding:10px; border-bottom:1px solid var(--border-color); cursor:pointer;" onclick="openProfile(${u.id})">
                    <div style="display:flex; align-items:center; gap:12px;">
                        <div class="user-circle"><img src="${u.img}"></div>
                        <b>${u.name}</b>
                    </div>
                    <button class="btn-main" style="width:auto; padding:5px 15px;">Follow</button>
                </div>
            `).join('');
        }

        function openProfile(id) {
            const u = systemUsers.find(x => x.id === id);
            document.getElementById('m-name').innerText = u.name;
            document.getElementById('m-img').src = u.img;
            document.getElementById('m-place').innerText = u.place;
            document.getElementById('m-edu').innerText = u.edu;
            document.getElementById('profile-modal').style.display = 'flex';
        }

        function saveSettings() {
            const n = document.getElementById('set_name').value;
            if(n) currentUser.name = n;
            localStorage.setItem('user', JSON.stringify(currentUser));
            alert("Sirreeffameera!");
            loadApp();
        }
    </script>
</body>
</html>
