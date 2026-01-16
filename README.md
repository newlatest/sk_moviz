<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MovieStream - Watch Movies & Series</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: #141414;
            color: #fff;
            overflow-x: hidden;
        }

        .header {
            position: fixed;
            top: 0;
            width: 100%;
            padding: 15px 4%;
            background: linear-gradient(180deg, rgba(0,0,0,0.8) 0%, transparent 100%);
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 1000;
            transition: background 0.3s;
        }

        .header.scrolled {
            background: #141414;
        }

        .logo {
            font-size: 28px;
            font-weight: bold;
            color: #e50914;
            cursor: pointer;
        }

        .nav-menu {
            display: flex;
            gap: 20px;
        }

        .nav-item {
            cursor: pointer;
            padding: 5px 10px;
            transition: color 0.3s;
        }

        .nav-item:hover, .nav-item.active {
            color: #e50914;
        }

        .user-actions {
            display: flex;
            gap: 15px;
            align-items: center;
        }

        .btn {
            padding: 8px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s;
        }

        .btn-primary {
            background: #e50914;
            color: white;
        }

        .btn-primary:hover {
            background: #f40612;
        }

        .btn-secondary {
            background: rgba(255,255,255,0.2);
            color: white;
        }

        .notice-banner {
            margin-top: 70px;
            background: linear-gradient(90deg, #e50914 0%, #831010 100%);
            padding: 15px 4%;
            display: none;
        }

        .notice-banner.active {
            display: block;
        }

        .notice-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .notice-text {
            font-size: 14px;
        }

        .notice-close {
            cursor: pointer;
            font-size: 20px;
            padding: 0 10px;
        }

        .hero {
            height: 70vh;
            background: linear-gradient(rgba(0,0,0,0.5), rgba(20,20,20,1)), url('https://images.unsplash.com/photo-1536440136628-849c177e76a1?w=1200') center/cover;
            display: flex;
            align-items: center;
            padding: 0 4%;
            margin-top: 60px;
        }

        .hero-content {
            max-width: 500px;
        }

        .hero-title {
            font-size: 48px;
            margin-bottom: 15px;
        }

        .hero-description {
            font-size: 16px;
            margin-bottom: 20px;
            line-height: 1.5;
        }

        .hero-buttons {
            display: flex;
            gap: 15px;
        }

        .btn-play {
            background: white;
            color: black;
            padding: 12px 30px;
            font-size: 16px;
            font-weight: bold;
        }

        .btn-info {
            background: rgba(255,255,255,0.3);
            color: white;
            padding: 12px 30px;
            font-size: 16px;
        }

        .container {
            padding: 40px 4%;
        }

        .section-title {
            font-size: 24px;
            margin-bottom: 20px;
            font-weight: bold;
        }

        .movie-row {
            overflow-x: auto;
            overflow-y: hidden;
            white-space: nowrap;
            padding-bottom: 10px;
            margin-bottom: 40px;
        }

        .movie-row::-webkit-scrollbar {
            height: 8px;
        }

        .movie-row::-webkit-scrollbar-track {
            background: #2a2a2a;
        }

        .movie-row::-webkit-scrollbar-thumb {
            background: #555;
            border-radius: 4px;
        }

        .movie-card {
            display: inline-block;
            width: 200px;
            margin-right: 10px;
            cursor: pointer;
            transition: transform 0.3s;
            position: relative;
        }

        .movie-card:hover {
            transform: scale(1.05);
        }

        .movie-poster {
            width: 100%;
            height: 300px;
            object-fit: cover;
            border-radius: 8px;
        }

        .movie-info {
            padding: 8px 0;
        }

        .movie-title {
            font-size: 14px;
            font-weight: bold;
            white-space: normal;
            line-height: 1.3;
        }

        .movie-meta {
            font-size: 12px;
            color: #999;
            margin-top: 5px;
        }

        .badge {
            position: absolute;
            top: 10px;
            right: 10px;
            background: #e50914;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 10px;
            font-weight: bold;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            z-index: 2000;
            overflow-y: auto;
        }

        .modal.active {
            display: block;
        }

        .modal-content {
            max-width: 900px;
            margin: 50px auto;
            background: #181818;
            border-radius: 8px;
            position: relative;
        }

        .modal-close {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 30px;
            cursor: pointer;
            z-index: 10;
            background: #181818;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .video-container {
            width: 100%;
            height: 500px;
            background: #000;
            border-radius: 8px 8px 0 0;
        }

        .video-player {
            width: 100%;
            height: 100%;
            border-radius: 8px 8px 0 0;
        }

        .movie-details {
            padding: 30px;
        }

        .movie-actions {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
        }

        .action-btn {
            background: rgba(255,255,255,0.1);
            border: 2px solid rgba(255,255,255,0.5);
            color: white;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .action-btn.active {
            background: #e50914;
            border-color: #e50914;
        }

        .detail-row {
            margin-bottom: 15px;
            line-height: 1.6;
        }

        .detail-label {
            color: #999;
            display: inline-block;
            width: 100px;
        }

        .bottom-nav {
            position: fixed;
            bottom: 0;
            width: 100%;
            background: #181818;
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            border-top: 1px solid #333;
            z-index: 1000;
        }

        .nav-btn {
            text-align: center;
            cursor: pointer;
            padding: 5px;
            flex: 1;
            color: #999;
        }

        .nav-btn.active {
            color: #e50914;
        }

        .nav-icon {
            font-size: 24px;
            margin-bottom: 5px;
        }

        .nav-label {
            font-size: 11px;
        }

        .search-container {
            padding: 80px 4% 20px;
        }

        .search-box {
            width: 100%;
            padding: 15px;
            background: #2a2a2a;
            border: none;
            border-radius: 8px;
            color: white;
            font-size: 16px;
            margin-bottom: 20px;
        }

        .admin-panel {
            padding: 80px 4% 80px;
        }

        .admin-login {
            max-width: 400px;
            margin: 100px auto;
            padding: 30px;
            background: #2a2a2a;
            border-radius: 8px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            font-size: 14px;
        }

        .form-input, .form-textarea, .form-select {
            width: 100%;
            padding: 12px;
            background: #333;
            border: 1px solid #555;
            border-radius: 4px;
            color: white;
            font-size: 14px;
        }

        .form-textarea {
            min-height: 100px;
            resize: vertical;
        }

        .admin-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            border-bottom: 2px solid #333;
        }

        .admin-tab {
            padding: 10px 20px;
            cursor: pointer;
            border-bottom: 3px solid transparent;
        }

        .admin-tab.active {
            border-bottom-color: #e50914;
            color: #e50914;
        }

        .admin-content {
            display: none;
        }

        .admin-content.active {
            display: block;
        }

        .item-list {
            margin-top: 20px;
        }

        .item-card {
            background: #2a2a2a;
            padding: 15px;
            margin-bottom: 15px;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .item-actions {
            display: flex;
            gap: 10px;
        }

        .btn-edit, .btn-delete {
            padding: 6px 12px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
        }

        .btn-edit {
            background: #4CAF50;
            color: white;
        }

        .btn-delete {
            background: #f44336;
            color: white;
        }

        .page {
            display: none;
            padding-bottom: 80px;
        }

        .page.active {
            display: block;
        }

        .empty-state {
            text-align: center;
            padding: 60px 20px;
            color: #999;
        }

        .empty-icon {
            font-size: 48px;
            margin-bottom: 20px;
        }

        @media (max-width: 768px) {
            .nav-menu {
                display: none;
            }

            .hero-title {
                font-size: 32px;
            }

            .movie-card {
                width: 150px;
            }

            .movie-poster {
                height: 225px;
            }

            .video-container {
                height: 300px;
            }
        }
    </style>
</head>
<body>
    <!-- Header -->
    <div class="header" id="header">
        <div class="logo">MovieStream</div>
        <div class="nav-menu">
            <div class="nav-item active" onclick="showPage('home')">Home</div>
            <div class="nav-item" onclick="showPage('browse')">Browse</div>
            <div class="nav-item" onclick="showPage('mylist')">My List</div>
        </div>
        <div class="user-actions">
            <button class="btn btn-secondary" onclick="showPage('search')">Search</button>
            <button class="btn btn-primary" onclick="showPage('admin')">Admin</button>
        </div>
    </div>

    <!-- Notice Banner -->
    <div class="notice-banner" id="noticeBanner">
        <div class="notice-content">
            <div class="notice-text" id="noticeText"></div>
            <div class="notice-close" onclick="closeNotice()">×</div>
        </div>
    </div>

    <!-- Home Page -->
    <div class="page active" id="homePage">
        <div class="hero">
            <div class="hero-content">
                <h1 class="hero-title">Featured Movie</h1>
                <p class="hero-description">Discover amazing movies and series. Start watching now!</p>
                <div class="hero-buttons">
                    <button class="btn btn-play">▶ Play</button>
                    <button class="btn btn-info">ℹ More Info</button>
                </div>
            </div>
        </div>

        <div class="container">
            <div class="section-title">Continue Watching</div>
            <div class="movie-row" id="continueWatching"></div>

            <div class="section-title">New Arrivals</div>
            <div class="movie-row" id="newArrivals"></div>

            <div class="section-title">Action Movies</div>
            <div class="movie-row" id="actionMovies"></div>

            <div class="section-title">Drama Series</div>
            <div class="movie-row" id="dramaSeries"></div>

            <div class="section-title">Comedy</div>
            <div class="movie-row" id="comedy"></div>
        </div>
    </div>

    <!-- Browse Page -->
    <div class="page" id="browsePage">
        <div class="container" style="margin-top: 80px;">
            <h1>Browse All Content</h1>
            <div id="browseContent"></div>
        </div>
    </div>

    <!-- Search Page -->
    <div class="page" id="searchPage">
        <div class="search-container">
            <input type="text" class="search-box" placeholder="Search for movies, series, actors..." id="searchInput" oninput="searchContent()">
            <div id="searchResults"></div>
        </div>
    </div>

    <!-- My List Page -->
    <div class="page" id="mylistPage">
        <div class="container" style="margin-top: 80px;">
            <h1>My List</h1>
            
            <div class="section-title">Watch Later</div>
            <div class="movie-row" id="watchLaterList"></div>

            <div class="section-title">Watched</div>
            <div class="movie-row" id="watchedList"></div>

            <div class="section-title">Favorites</div>
            <div class="movie-row" id="favoritesList"></div>
        </div>
    </div>

    <!-- Admin Page -->
    <div class="page" id="adminPage">
        <div class="admin-login" id="adminLogin">
            <h2 style="margin-bottom: 20px;">Admin Login</h2>
            <div class="form-group">
                <label class="form-label">Password</label>
                <input type="password" class="form-input" id="adminPassword">
            </div>
            <button class="btn btn-primary" style="width: 100%;" onclick="adminLogin()">Login</button>
        </div>

        <div class="admin-panel" id="adminPanel" style="display: none;">
            <h1>Admin Panel</h1>
            
            <div class="admin-tabs">
                <div class="admin-tab active" onclick="switchAdminTab('movies')">Manage Movies</div>
                <div class="admin-tab" onclick="switchAdminTab('notices')">Manage Notices</div>
            </div>

            <!-- Movies Management -->
            <div class="admin-content active" id="moviesTab">
                <h2>Add New Movie/Series</h2>
                <form onsubmit="addMovie(event)">
                    <div class="form-group">
                        <label class="form-label">Title</label>
                        <input type="text" class="form-input" id="movieTitle" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Type</label>
                        <select class="form-select" id="movieType">
                            <option value="movie">Movie</option>
                            <option value="series">Series</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Poster URL</label>
                        <input type="url" class="form-input" id="moviePoster" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Video URL</label>
                        <input type="url" class="form-input" id="movieVideo" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Genre</label>
                        <select class="form-select" id="movieGenre">
                            <option value="action">Action</option>
                            <option value="drama">Drama</option>
                            <option value="comedy">Comedy</option>
                            <option value="horror">Horror</option>
                            <option value="romance">Romance</option>
                            <option value="sci-fi">Sci-Fi</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Year</label>
                        <input type="number" class="form-input" id="movieYear" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Rating (1-10)</label>
                        <input type="number" class="form-input" id="movieRating" min="1" max="10" step="0.1" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Duration (minutes)</label>
                        <input type="number" class="form-input" id="movieDuration" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Description</label>
                        <textarea class="form-textarea" id="movieDescription" required></textarea>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Cast (comma separated)</label>
                        <input type="text" class="form-input" id="movieCast" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Director</label>
                        <input type="text" class="form-input" id="movieDirector" required>
                    </div>
                    <button type="submit" class="btn btn-primary">Add Movie/Series</button>
                </form>

                <div class="item-list" id="moviesList"></div>
            </div>

            <!-- Notices Management -->
            <div class="admin-content" id="noticesTab">
                <h2>Add New Notice</h2>
                <form onsubmit="addNotice(event)">
                    <div class="form-group">
                        <label class="form-label">Notice Message</label>
                        <textarea class="form-textarea" id="noticeMessage" required></textarea>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Priority</label>
                        <select class="form-select" id="noticePriority">
                            <option value="info">Info</option>
                            <option value="normal">Normal</option>
                            <option value="urgent">Urgent</option>
                        </select>
                    </div>
                    <button type="submit" class="btn btn-primary">Add Notice</button>
                </form>

               <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MovieStream - Watch Movies & Series</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: #141414;
            color: #fff;
            overflow-x: hidden;
        }

        .header {
            position: fixed;
            top: 0;
            width: 100%;
            padding: 15px 4%;
            background: linear-gradient(180deg, rgba(0,0,0,0.8) 0%, transparent 100%);
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 1000;
            transition: background 0.3s;
        }

        .header.scrolled {
            background: #141414;
        }

        .logo {
            font-size: 28px;
            font-weight: bold;
            color: #e50914;
            cursor: pointer;
        }

        .nav-menu {
            display: flex;
            gap: 20px;
        }

        .nav-item {
            cursor: pointer;
            padding: 5px 10px;
            transition: color 0.3s;
        }

        .nav-item:hover, .nav-item.active {
            color: #e50914;
        }

        .user-actions {
            display: flex;
            gap: 15px;
            align-items: center;
        }

        .btn {
            padding: 8px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s;
        }

        .btn-primary {
            background: #e50914;
            color: white;
        }

        .btn-primary:hover {
            background: #f40612;
        }

        .btn-secondary {
            background: rgba(255,255,255,0.2);
            color: white;
        }

        .notice-banner {
            margin-top: 70px;
            background: linear-gradient(90deg, #e50914 0%, #831010 100%);
            padding: 15px 4%;
            display: none;
        }

        .notice-banner.active {
            display: block;
        }

        .notice-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .notice-text {
            font-size: 14px;
        }

        .notice-close {
            cursor: pointer;
            font-size: 20px;
            padding: 0 10px;
        }

        .hero {
            height: 70vh;
            background: linear-gradient(rgba(0,0,0,0.5), rgba(20,20,20,1)), url('https://images.unsplash.com/photo-1536440136628-849c177e76a1?w=1200') center/cover;
            display: flex;
            align-items: center;
            padding: 0 4%;
            margin-top: 60px;
        }

        .hero-content {
            max-width: 500px;
        }

        .hero-title {
            font-size: 48px;
            margin-bottom: 15px;
        }

        .hero-description {
            font-size: 16px;
            margin-bottom: 20px;
            line-height: 1.5;
        }

        .hero-buttons {
            display: flex;
            gap: 15px;
        }

        .btn-play {
            background: white;
            color: black;
            padding: 12px 30px;
            font-size: 16px;
            font-weight: bold;
        }

        .btn-info {
            background: rgba(255,255,255,0.3);
            color: white;
            padding: 12px 30px;
            font-size: 16px;
        }

        .container {
            padding: 40px 4%;
        }

        .section-title {
            font-size: 24px;
            margin-bottom: 20px;
            font-weight: bold;
        }

        .movie-row {
            overflow-x: auto;
            overflow-y: hidden;
            white-space: nowrap;
            padding-bottom: 10px;
            margin-bottom: 40px;
        }

        .movie-row::-webkit-scrollbar {
            height: 8px;
        }

        .movie-row::-webkit-scrollbar-track {
            background: #2a2a2a;
        }

        .movie-row::-webkit-scrollbar-thumb {
            background: #555;
            border-radius: 4px;
        }

        .movie-card {
            display: inline-block;
            width: 200px;
            margin-right: 10px;
            cursor: pointer;
            transition: transform 0.3s;
            position: relative;
        }

        .movie-card:hover {
            transform: scale(1.05);
        }

        .movie-poster {
            width: 100%;
            height: 300px;
            object-fit: cover;
            border-radius: 8px;
        }

        .movie-info {
            padding: 8px 0;
        }

        .movie-title {
            font-size: 14px;
            font-weight: bold;
            white-space: normal;
            line-height: 1.3;
        }

        .movie-meta {
            font-size: 12px;
            color: #999;
            margin-top: 5px;
        }

        .badge {
            position: absolute;
            top: 10px;
            right: 10px;
            background: #e50914;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 10px;
            font-weight: bold;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            z-index: 2000;
            overflow-y: auto;
        }

        .modal.active {
            display: block;
        }

        .modal-content {
            max-width: 900px;
            margin: 50px auto;
            background: #181818;
            border-radius: 8px;
            position: relative;
        }

        .modal-close {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 30px;
            cursor: pointer;
            z-index: 10;
            background: #181818;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .video-container {
            width: 100%;
            height: 500px;
            background: #000;
            border-radius: 8px 8px 0 0;
        }

        .video-player {
            width: 100%;
            height: 100%;
            border-radius: 8px 8px 0 0;
        }

        .movie-details {
            padding: 30px;
        }

        .movie-actions {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
        }

        .action-btn {
            background: rgba(255,255,255,0.1);
            border: 2px solid rgba(255,255,255,0.5);
            color: white;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .action-btn.active {
            background: #e50914;
            border-color: #e50914;
        }

        .detail-row {
            margin-bottom: 15px;
            line-height: 1.6;
        }

        .detail-label {
            color: #999;
            display: inline-block;
            width: 100px;
        }

        .bottom-nav {
            position: fixed;
            bottom: 0;
            width: 100%;
            background: #181818;
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            border-top: 1px solid #333;
            z-index: 1000;
        }

        .nav-btn {
            text-align: center;
            cursor: pointer;
            padding: 5px;
            flex: 1;
            color: #999;
        }

        .nav-btn.active {
            color: #e50914;
        }

        .nav-icon {
            font-size: 24px;
            margin-bottom: 5px;
        }

        .nav-label {
            font-size: 11px;
        }

        .search-container {
            padding: 80px 4% 20px;
        }

        .search-box {
            width: 100%;
            padding: 15px;
            background: #2a2a2a;
            border: none;
            border-radius: 8px;
            color: white;
            font-size: 16px;
            margin-bottom: 20px;
        }

        .admin-panel {
            padding: 80px 4% 80px;
        }

        .admin-login {
            max-width: 400px;
            margin: 100px auto;
            padding: 30px;
            background: #2a2a2a;
            border-radius: 8px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            font-size: 14px;
        }

        .form-input, .form-textarea, .form-select {
            width: 100%;
            padding: 12px;
            background: #333;
            border: 1px solid #555;
            border-radius: 4px;
            color: white;
            font-size: 14px;
        }

        .form-textarea {
            min-height: 100px;
            resize: vertical;
        }

        .admin-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            border-bottom: 2px solid #333;
        }

        .admin-tab {
            padding: 10px 20px;
            cursor: pointer;
            border-bottom: 3px solid transparent;
        }

        .admin-tab.active {
            border-bottom-color: #e50914;
            color: #e50914;
        }

        .admin-content {
            display: none;
        }

        .admin-content.active {
            display: block;
        }

        .item-list {
            margin-top: 20px;
        }

        .item-card {
            background: #2a2a2a;
            padding: 15px;
            margin-bottom: 15px;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .item-actions {
            display: flex;
            gap: 10px;
        }

        .btn-edit, .btn-delete {
            padding: 6px 12px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
        }

        .btn-edit {
            background: #4CAF50;
            color: white;
        }

        .btn-delete {
            background: #f44336;
            color: white;
        }

        .page {
            display: none;
            padding-bottom: 80px;
        }

        .page.active {
            display: block;
        }

        .empty-state {
            text-align: center;
            padding: 60px 20px;
            color: #999;
        }

        .empty-icon {
            font-size: 48px;
            margin-bottom: 20px;
        }

        @media (max-width: 768px) {
            .nav-menu {
                display: none;
            }

            .hero-title {
                font-size: 32px;
            }

            .movie-card {
                width: 150px;
            }

            .movie-poster {
                height: 225px;
            }

            .video-container {
                height: 300px;
            }
        }
    </style>
</head>
<body>
    <!-- Header -->
    <div class="header" id="header">
        <div class="logo">MovieStream</div>
        <div class="nav-menu">
            <div class="nav-item active" onclick="showPage('home')">Home</div>
            <div class="nav-item" onclick="showPage('browse')">Browse</div>
            <div class="nav-item" onclick="showPage('mylist')">My List</div>
        </div>
        <div class="user-actions">
            <button class="btn btn-secondary" onclick="showPage('search')">Search</button>
            <button class="btn btn-primary" onclick="showPage('admin')">Admin</button>
        </div>
    </div>

    <!-- Notice Banner -->
    <div class="notice-banner" id="noticeBanner">
        <div class="notice-content">
            <div class="notice-text" id="noticeText"></div>
            <div class="notice-close" onclick="closeNotice()">×</div>
        </div>
    </div>

    <!-- Home Page -->
    <div class="page active" id="homePage">
        <div class="hero">
            <div class="hero-content">
                <h1 class="hero-title">Featured Movie</h1>
                <p class="hero-description">Discover amazing movies and series. Start watching now!</p>
                <div class="hero-buttons">
                    <button class="btn btn-play">▶ Play</button>
                    <button class="btn btn-info">ℹ More Info</button>
                </div>
            </div>
        </div>

        <div class="container">
            <div class="section-title">Continue Watching</div>
            <div class="movie-row" id="continueWatching"></div>

            <div class="section-title">New Arrivals</div>
            <div class="movie-row" id="newArrivals"></div>

            <div class="section-title">Action Movies</div>
            <div class="movie-row" id="actionMovies"></div>

            <div class="section-title">Drama Series</div>
            <div class="movie-row" id="dramaSeries"></div>

            <div class="section-title">Comedy</div>
            <div class="movie-row" id="comedy"></div>
        </div>
    </div>

    <!-- Browse Page -->
    <div class="page" id="browsePage">
        <div class="container" style="margin-top: 80px;">
            <h1>Browse All Content</h1>
            <div id="browseContent"></div>
        </div>
    </div>

    <!-- Search Page -->
    <div class="page" id="searchPage">
        <div class="search-container">
            <input type="text" class="search-box" placeholder="Search for movies, series, actors..." id="searchInput" oninput="searchContent()">
            <div id="searchResults"></div>
        </div>
    </div>

    <!-- My List Page -->
    <div class="page" id="mylistPage">
        <div class="container" style="margin-top: 80px;">
            <h1>My List</h1>
            
            <div class="section-title">Watch Later</div>
            <div class="movie-row" id="watchLaterList"></div>

            <div class="section-title">Watched</div>
            <div class="movie-row" id="watchedList"></div>

            <div class="section-title">Favorites</div>
            <div class="movie-row" id="favoritesList"></div>
        </div>
    </div>

    <!-- Admin Page -->
    <div class="page" id="adminPage">
        <div class="admin-login" id="adminLogin">
            <h2 style="margin-bottom: 20px;">Admin Login</h2>
            <div class="form-group">
                <label class="form-label">Password</label>
                <input type="password" class="form-input" id="adminPassword">
            </div>
            <button class="btn btn-primary" style="width: 100%;" onclick="adminLogin()">Login</button>
        </div>

        <div class="admin-panel" id="adminPanel" style="display: none;">
            <h1>Admin Panel</h1>
            
            <div class="admin-tabs">
                <div class="admin-tab active" onclick="switchAdminTab('movies')">Manage Movies</div>
                <div class="admin-tab" onclick="switchAdminTab('notices')">Manage Notices</div>
            </div>

            <!-- Movies Management -->
            <div class="admin-content active" id="moviesTab">
                <h2>Add New Movie/Series</h2>
                <form onsubmit="addMovie(event)">
                    <div class="form-group">
                        <label class="form-label">Title</label>
                        <input type="text" class="form-input" id="movieTitle" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Type</label>
                        <select class="form-select" id="movieType">
                            <option value="movie">Movie</option>
                            <option value="series">Series</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Poster URL</label>
                        <input type="url" class="form-input" id="moviePoster" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Video URL</label>
                        <input type="url" class="form-input" id="movieVideo" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Genre</label>
                        <select class="form-select" id="movieGenre">
                            <option value="action">Action</option>
                            <option value="drama">Drama</option>
                            <option value="comedy">Comedy</option>
                            <option value="horror">Horror</option>
                            <option value="romance">Romance</option>
                            <option value="sci-fi">Sci-Fi</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Year</label>
                        <input type="number" class="form-input" id="movieYear" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Rating (1-10)</label>
                        <input type="number" class="form-input" id="movieRating" min="1" max="10" step="0.1" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Duration (minutes)</label>
                        <input type="number" class="form-input" id="movieDuration" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Description</label>
                        <textarea class="form-textarea" id="movieDescription" required></textarea>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Cast (comma separated)</label>
                        <input type="text" class="form-input" id="movieCast" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Director</label>
                        <input type="text" class="form-input" id="movieDirector" required>
                    </div>
                    <button type="submit" class="btn btn-primary">Add Movie/Series</button>
                </form>

                <div class="item-list" id="moviesList"></div>
            </div>

            <!-- Notices Management -->
            <div class="admin-content" id="noticesTab">
                <h2>Add New Notice</h2>
                <form onsubmit="addNotice(event)">
                    <div class="form-group">
                        <label class="form-label">Notice Message</label>
                        <textarea class="form-textarea" id="noticeMessage" required></textarea>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Priority</label>
                        <select class="form-select" id="noticePriority">
                            <option value="info">Info</option>
                            <option value="normal">Normal</option>
                            <option value="urgent">Urgent</option>
                        </select>
                    </div>
                    <button type="submit" class="btn btn-primary">Add Notice</button>
                </form>

               
