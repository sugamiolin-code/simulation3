<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>鉄骨建方 3Dモデル確認資料</title>
    <style>
        :root{--bg:#f4f6f8;--panel:#ffffff;--ink:#1f2933;--muted:#6b7280;--line:#e5e7eb;--accent:#1e88e5;--accent2:#e53935;}
        *{box-sizing:border-box}
        html,body{margin:0;height:100%;overflow:hidden;font-family:system-ui,sans-serif;color:var(--ink);background:var(--bg);font-size:14px}
        #app{display:grid;grid-template-columns:380px 1fr;grid-template-rows:100vh;height:100vh}
        #side{background:var(--panel);border-right:1px solid var(--line);display:flex;flex-direction:column;height:100vh}
        #side header{padding:14px 16px 10px;border-bottom:1px solid var(--line)}
        #side header h1{font-size:15px;margin:0}
        #issues{overflow:auto;flex:1;padding:8px}
        .card{border:1px solid var(--line);border-radius:8px;padding:10px 12px;margin:0 0 8px;background:#fff}
        .card .ttl{font-weight:700;font-size:13.5px;margin-bottom:4px}
        .meta{display:flex;gap:6px;margin-top:6px}
        .chip{font-size:11px;border-radius:999px;padding:1px 8px;background:#eef2f7}
        #main{position:relative;background:#eee}
        #view{width:100%;height:100%}
    </style>
    <!-- 3Dライブラリの読み込み -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/OBJLoader.js"></script>
</head>
<body>
<div id="app">
    <aside id="side">
        <header>
            <h1>建物モデル・要素確認</h1>
        </header>
        <div id="issues">
            <!-- 対策内容（.sum）を削除したカード構成 -->
            <div class="card">
                <div class="ttl">建物外観・全体構造</div>
                <div class="meta">
                    <span class="chip">OBJモデル</span>
                    <span class="chip">要素数: 1250</span>
                </div>
            </div>
            <div class="card">
                <div class="ttl">主要構造部（柱・梁）</div>
                <div class="meta">
                    <span class="chip">OBJモデル</span>
                    <span class="chip">要素数: 840</span>
                </div>
            </div>
        </div>
    </aside>
    <main id="main">
        <div id="view"></div>
    </main>
</div>

<script>
    const container = document.getElementById('view');
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0xf4f6f8);
    
    const camera = new THREE.PerspectiveCamera(45, container.clientWidth / container.clientHeight, 0.1, 1000);
    camera.position.set(20, 20, 20);

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(container.clientWidth, container.clientHeight);
    container.appendChild(renderer.domElement);

    scene.add(new THREE.AmbientLight(0xffffff, 0.7));
    const dirLight = new THREE.DirectionalLight(0xffffff, 0.8);
    dirLight.position.set(5, 10, 7);
    scene.add(dirLight);

    // ★新しい建物モデルの読み込み設定★
    const loader = new THREE.OBJLoader();
    loader.load(
        'new_building.obj', // 2FL.obj
        function (object) {
            scene.add(object);
        },
        undefined,
        function (error) { console.error('モデル読み込み失敗', error); }
    );

    function animate() {
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
    }
    animate();

    window.addEventListener('resize', () => {
        camera.aspect = container.clientWidth / container.clientHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(container.clientWidth, container.clientHeight);
    });
</script>
</body>
</html>
