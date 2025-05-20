<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CKG News - Modern News Experience</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        /* Modern Design System */
        :root {
            --primary: #FF3B30;
            --secondary: #007AFF;
            --background: #FFFFFF;
            --surface: #F2F2F7;
            --on-primary: #FFFFFF;
            --on-background: #1C1C1E;
            --on-surface: #3A3A3C;
            --border-radius: 16px;
            --shadow: 0 8px 24px rgba(0,0,0,0.08);
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        [data-theme="dark"] {
            --background: #1C1C1E;
            --surface: #2C2C2E;
            --on-background: #FFFFFF;
            --on-surface: #EBEBF5;
        }

        /* Base Styles */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-font-smoothing: antialiased;
        }

        body {
            background: var(--background);
            color: var(--on-background);
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            line-height: 1.6;
            transition: var(--transition);
        }

        /* Modern Header */
        .app-bar {
            position: fixed;
            top: 0;
            width: 100%;
            background: var(--background);
            padding: 1rem 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: var(--shadow);
            z-index: 1000;
        }

        .logo {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--primary);
        }

        /* News Grid */
        .news-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
            padding: 2rem;
            margin-top: 80px;
        }

        .article-card {
            background: var(--surface);
            border-radius: var(--border-radius);
            overflow: hidden;
            transition: var(--transition);
            position: relative;
        }

        .article-card:hover {
            transform: translateY(-4px);
            box-shadow: var(--shadow);
        }

        .article-image {
            width: 100%;
            height: 200px;
            object-fit: cover;
            border-radius: var(--border-radius) var(--border-radius) 0 0;
        }

        .article-content {
            padding: 1.5rem;
        }

        .article-category {
            position: absolute;
            top: 1rem;
            left: 1rem;
            background: var(--primary);
            color: var(--on-primary);
            padding: 0.25rem 0.75rem;
            border-radius: 20px;
            font-size: 0.875rem;
        }

        /* Floating Action Button */
        .fab {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            background: var(--primary);
            color: var(--on-primary);
            width: 56px;
            height: 56px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: var(--shadow);
            cursor: pointer;
            transition: var(--transition);
        }

        /* Modern Admin Panel */
        .admin-panel {
            background: var(--surface);
            border-radius: var(--border-radius);
            padding: 2rem;
            margin: 2rem;
            box-shadow: var(--shadow);
        }

        .form-control {
            margin-bottom: 1.5rem;
        }

        .form-control input,
        .form-control textarea {
            width: 100%;
            padding: 1rem;
            background: var(--background);
            border: 1px solid rgba(0,0,0,0.1);
            border-radius: 8px;
            transition: var(--transition);
        }

        /* Theme Toggle */
        .theme-toggle {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            padding: 0.5rem 1rem;
            border-radius: 8px;
            background: var(--surface);
            cursor: pointer;
        }

        /* Loading Skeleton */
        .skeleton {
            background: var(--surface);
            border-radius: var(--border-radius);
            animation: skeleton-loading 1.5s infinite;
        }

        @keyframes skeleton-loading {
            0% { opacity: 0.6 }
            50% { opacity: 1 }
            100% { opacity: 0.6 }
        }

        /* Modern Dialog */
        .dialog {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: var(--background);
            padding: 2rem;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            max-width: 500px;
            width: 90%;
            z-index: 2000;
        }

        .dialog-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1999;
        }
    </style>
</head>
<body>
    <!-- App Bar -->
    <header class="app-bar">
        <div class="logo">CKG News</div>
        <nav class="nav-items">
            <button class="theme-toggle" onclick="toggleTheme()">
                <i class="fas fa-moon"></i>
                <span>Theme</span>
            </button>
        </nav>
    </header>

    <!-- Main Content -->
    <main>
        <div class="news-grid" id="newsContainer">
            <!-- Dynamic Content -->
        </div>

        <!-- Floating Action Button -->
        <div class="fab" onclick="showEditor()">
            <i class="fas fa-plus"></i>
        </div>
    </main>

    <!-- Article Editor Dialog -->
    <div class="dialog-overlay" id="editorOverlay" style="display: none;">
        <div class="dialog">
            <h2>Create New Article</h2>
            <form id="articleForm" onsubmit="handleSubmit(event)">
                <div class="form-control">
                    <input type="text" placeholder="Article Title" required>
                </div>
                <div class="form-control">
                    <textarea placeholder="Content" rows="6" required></textarea>
                </div>
                <div class="form-control">
                    <input type="file" accept="image/*" id="imageUpload">
                </div>
                <div class="form-actions">
                    <button type="submit" class="primary-btn">Publish</button>
                    <button type="button" onclick="hideEditor()">Cancel</button>
                </div>
            </form>
        </div>
    </div>

    <script>
        // Modern UI Interactions
        function toggleTheme() {
            document.documentElement.setAttribute('data-theme',
                document.documentElement.getAttribute('data-theme') === 'light' ? 'dark' : 'light'
            );
        }

        function showEditor() {
            document.getElementById('editorOverlay').style.display = 'block';
        }

        function hideEditor() {
            document.getElementById('editorOverlay').style.display = 'none';
        }

        // Enhanced Article Handling
        class ArticleManager {
            constructor() {
                this.articles = JSON.parse(localStorage.getItem('articles')) || [];
                this.categories = ['Politics', 'Technology', 'Sports'];
            }

            saveArticle(article) {
                this.articles.unshift(article);
                localStorage.setItem('articles', JSON.stringify(this.articles));
                this.renderArticles();
            }

            async handleImageUpload(file) {
                // Simulated upload delay
                return new Promise(resolve => {
                    setTimeout(() => {
                        resolve(URL.createObjectURL(file));
                    }, 1000);
                });
            }

            renderArticles() {
                const container = document.getElementById('newsContainer');
                container.innerHTML = this.articles.map(article => `
                    <article class="article-card">
                        <div class="article-category">${article.category}</div>
                        <img src="${article.image}" class="article-image" loading="lazy">
                        <div class="article-content">
                            <h3>${article.title}</h3>
                            <p>${article.content.substring(0, 100)}...</p>
                            <div class="article-meta">
                                <time>${new Date(article.date).toLocaleDateString()}</time>
                            </div>
                        </div>
                    </article>
                `).join('');
            }
        }

        // Initialize App
        const newsApp = new ArticleManager();
        newsApp.renderArticles();

        // Form Handling
        async function handleSubmit(e) {
            e.preventDefault();
            const formData = new FormData(e.target);
            const imageFile = document.getElementById('imageUpload').files[0];
            
            const newArticle = {
                title: formData.get('title'),
                content: formData.get('content'),
                category: 'General',
                date: new Date().toISOString(),
                image: await newsApp.handleImageUpload(imageFile)
            };

            newsApp.saveArticle(newArticle);
            hideEditor();
        }

        // Progressive Enhancement
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('/sw.js');
        }

        // Dynamic Imports
        if (window.matchMedia('(max-width: 768px)').matches) {
            import('./mobile-ux.js');
        }
    </script>
</body>
</html>
