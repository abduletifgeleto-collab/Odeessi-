function showHomePage() {
    document.getElementById('login-page').style.display = 'none';
    document.getElementById('main-app').style.display = 'flex';
    loadUsers();
}

function showView(id) {
    document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    event.currentTarget.classList.add('active');
}

function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
}

function addNewPost() {
    const text = document.getElementById('postText').value;
    if(!text) return;
    const feeds = document.getElementById('feeds');
    const post = document.createElement('div');
    post.className = 'card';
    post.innerHTML = `
        <div style="display:flex; align-items:center; gap:10px; margin-bottom:10px;">
            <div style="width:40px; height:40px; background:#ccc; border-radius:50%;"></div>
            <b>Maqaa Kee</b>
        </div>
        <p style="font-size:16px;">${text}</p>
        <div style="border-top:1px solid #ddd; margin-top:10px; padding-top:10px; display:flex; gap:20px; color:#65676b;">
            <span><i class="fas fa-thumbs-up"></i> Like</span>
            <span><i class="fas fa-comment"></i> Comment</span>
        </div>`;
    feeds.prepend(post);
    document.getElementById('postText').value = "";
}

function loadUsers() {
    const names = ["Obsaa D.", "Biiftuu A.", "Lammaa M.", "Caalaa B."];
    const container = document.getElementById('user-list-box');
    container.innerHTML = "";
    names.forEach(n => {
        container.innerHTML += `
            <div class="card flex-row" style="display:flex; justify-content:space-between; align-items:center;">
                <b>${n}</b>
                <button class="btn-post" style="padding:5px 15px;">Add Friend</button>
            </div>`;
    });
}
root {
    --primary: #1877f2; /* Facebook Blue */
    --bg: #f0f2f5;
    --card: #ffffff;
    --text: #1c1e21;
    --sub: #65676b;
    --border: #dddfe2;
}

.dark-mode {
    --bg: #18191a;
    --card: #242526;
    --text: #e4e6eb;
    --sub: #b0b3b8;
    --border: #3e4042;
}

* { margin:0; padding:0; box-sizing:border-box; font-family: 'Segoe UI', Helvetica, Arial, sans-serif; }
body { background: var(--bg); color: var(--text); transition: 0.3s; font-size: 15px; }

/* Global UI */
.page { height: 100vh; display: flex; }
.card { background: var(--card); border-radius: 8px; padding: 15px; margin-bottom: 15px; box-shadow: 0 1px 2px rgba(0,0,0,0.1); }

/* Login Page */
#login-page { justify-content: center; align-items: center; background: #f0f2f5; }
.login-card { background: white; padding: 40px; border-radius: 8px; width: 400px; text-align: center; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
.brand-logo { color: var(--primary); font-size: 50px; font-weight: bold; margin-bottom: 20px; }
.btn-main { background: var(--primary); color: white; border: none; padding: 12px; width: 100%; border-radius: 6px; font-size: 20px; font-weight: bold; cursor: pointer; }

/* Sidebar */
.sidebar { width: 280px; background: var(--card); border-right: 1px solid var(--border); display: flex; flex-direction: column; padding: 10px; }
.sidebar-header { font-size: 24px; font-weight: bold; color: var(--primary); padding: 15px; }
.nav-item { padding: 12px 15px; border-radius: 8px; display: flex; align-items: center; gap: 15px; cursor: pointer; color: var(--text); font-weight: 600; }
.nav-item:hover, .nav-item.active { background: rgba(0,0,0,0.05); }
.nav-item i { font-size: 22px; width: 30px; }

/* Top Nav */
.top-nav { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
.search-box { background: #e4e6eb; padding: 10px 15px; border-radius: 20px; display: flex; align-items: center; gap: 10px; width: 350px; }
.search-box input { border: none; background: none; outline: none; font-size: 15px; width: 100%; }

/* Home & Stories */
.stories { display: flex; gap: 10px; margin-bottom: 20px; }
.story-box { width: 110px; height: 190px; border-radius: 10px; background: #ccc; cursor: pointer; }
.add { background: var(--card); display: flex; justify-content: center; align-items: center; font-size: 30px; color: var(--primary); border: 1px solid var(--border); }

/* Post Editor */
.post-editor textarea { flex: 1; border: none; background: #f0f2f5; border-radius: 20px; padding: 10px 15px; resize: none; outline: none; font-size: 16px; margin-left: 10px; }
.post-tools { display: flex; justify-content: space-between; border-top: 1px solid var(--border); margin-top: 15px; padding-top: 10px; }
.btn-post { background: var(--primary); color: white; border: none; padding: 6px 20px; border-radius: 6px; font-weight: bold; }

/* Settings Grid */
.settings-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 10px; }
.s-field label { font-size: 13px; color: var(--sub); display: block; margin-bottom: 5px; }
input, select { width: 100%; padding: 10px; border-radius: 6px; border: 1px solid var(--border); background: var(--bg); color: var(--text); }

.view { display: none; max-width: 700px; margin: 0 auto; }
.view.active { display: block; }
function showHomePage() {
    document.getElementById('login-page').style.display = 'none';
    document.getElementById('main-app').style.display = 'flex';
    loadUsers();
}

function showView(id) {
    document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    event.currentTarget.classList.add('active');
}

function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
}

function addNewPost() {
    const text = document.getElementById('postText').value;
    if(!text) return;
    const feeds = document.getElementById('feeds');
    const post = document.createElement('div');
    post.className = 'card';
    post.innerHTML = `
        <div style="display:flex; align-items:center; gap:10px; margin-bottom:10px;">
            <div style="width:40px; height:40px; background:#ccc; border-radius:50%;"></div>
            <b>Maqaa Kee</b>
        </div>
        <p style="font-size:16px;">${text}</p>
        <div style="border-top:1px solid #ddd; margin-top:10px; padding-top:10px; display:flex; gap:20px; color:#65676b;">
            <span><i class="fas fa-thumbs-up"></i> Like</span>
            <span><i class="fas fa-comment"></i> Comment</span>
        </div>`;
    feeds.prepend(post);
    document.getElementById('postText').value = "";
}

function loadUsers() {
    const names = ["Obsaa D.", "Biiftuu A.", "Lammaa M.", "Caalaa B."];
    const container = document.getElementById('user-list-box');
    container.innerHTML = "";
    names.forEach(n => {
        container.innerHTML += `
            <div class="card flex-row" style="display:flex; justify-content:space-between; align-items:center;">
                <b>${n}</b>
                <button class="btn-post" style="padding:5px 15px;">Add Friend</button>
            </div>`;
    });
}
