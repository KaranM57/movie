<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CineFlix - Movie Hub</title>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: #101014;
  color: white;
  font-family: Arial, sans-serif;
}

header {
  background: #1b1b22;
  padding: 18px 5%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 15px;
  flex-wrap: wrap;
}

.logo {
  color: #ff304f;
  font-size: 27px;
  font-weight: bold;
}

nav a {
  color: white;
  text-decoration: none;
  margin: 0 8px;
}

.search {
  padding: 10px;
  border-radius: 6px;
  border: 1px solid #444;
  background: #101014;
  color: white;
  max-width: 100%;
}

.hero {
  padding: 65px 6%;
  background: linear-gradient(90deg, #171720, #48202e);
}

.hero h1 {
  font-size: clamp(30px, 5vw, 48px);
  margin-bottom: 15px;
}

.hero p {
  color: #ddd;
  max-width: 500px;
  line-height: 1.6;
  margin-bottom: 22px;
}

.button {
  background: #ff304f;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  display: inline-block;
}

main {
  padding: 30px 5%;
}

.section-title {
  margin-bottom: 20px;
}

.categories {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin-bottom: 25px;
}

.category {
  background: #25252e;
  color: white;
  border: 1px solid #41414b;
  padding: 9px 15px;
  border-radius: 20px;
  cursor: pointer;
}

.category.active {
  background: #ff304f;
  border-color: #ff304f;
}

.movies {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(145px, 1fr));
  gap: 20px;
}

.movie {
  background: #1b1b22;
  border-radius: 9px;
  overflow: hidden;
  transition: transform 0.2s;
}

.movie:hover {
  transform: translateY(-5px);
}

.poster {
  height: 210px;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 15px;
  font-size: 22px;
  font-weight: bold;
  background: linear-gradient(145deg, #4b244f, #171720);
}

.movie-info {
  padding: 12px;
}

.movie-info h3 {
  font-size: 15px;
  margin-bottom: 7px;
}

.movie-info p {
  color: #aaa;
  font-size: 13px;
  margin-bottom: 12px;
}

.movie-info a {
  font-size: 13px;
  color: #ff6178;
  text-decoration: none;
}

.empty {
  display: none;
  color: #bbb;
  padding: 25px 0;
}

footer {
  text-align: center;
  padding: 25px;
  color: #999;
  background: #1b1b22;
  margin-top: 35px;
}

@media (max-width: 600px) {
  header {
    justify-content: center;
  }

  nav {
    width: 100%;
    text-align: center;
  }

  .search {
    width: 100%;
  }

  .hero {
    padding: 45px 6%;
  }

  .movies {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 12px;
  }

  .poster {
    height: 190px;
  }
}
</style>
</head>

<body>

<header>
  <div class="logo">CineFlix</div>

  <nav>
    <a href="#">Home</a>
    <a href="#movies">Movies</a>
    <a href="#categories">Genres</a>
  </nav>

  <input
    class="search"
    id="search"
    type="search"
    placeholder="Search movies..."
    aria-label="Search movies"
  >
</header>

<section class="hero">
  <h1>Your Movie Universe</h1>
  <p>
    Discover exciting films, explore new stories, and find
    your next favorite movie in one place.
  </p>
  <a href="#movies" class="button">Explore Movies</a>
</section>

<main>
  <h2 class="section-title" id="categories">Browse Genres</h2>

  <div class="categories">
    <button class="category active" data-genre="All">All</button>
    <button class="category" data-genre="Action">Action</button>
    <button class="category" data-genre="Drama">Drama</button>
    <button class="category" data-genre="Comedy">Comedy</button>
    <button class="category" data-genre="Sci-Fi">Sci-Fi</button>
  </div>

  <h2 class="section-title" id="movies">Popular Movies</h2>

  <div class="movies" id="movieList"></div>
  <p class="empty" id="empty">No movies found. Try another search.</p>
</main>

<footer>
  <p>© 2026 CineFlix · Movie discovery and official trailers</p>
</footer>

<script>
const movies = [
  { title: "Dark Horizon", year: 2026, genre: "Action", color: "#662b3a" },
  { title: "City of Dreams", year: 2025, genre: "Drama", color: "#314c68" },
  { title: "Funny Business", year: 2025, genre: "Comedy", color: "#73562c" },
  { title: "Beyond Earth", year: 2026, genre: "Sci-Fi", color: "#274f59" },
  { title: "The Final Chase", year: 2024, genre: "Action", color: "#503a70" },
  { title: "Silent Roads", year: 2025, genre: "Drama", color: "#5c453d" },
  { title: "Weekend Chaos", year: 2024, genre: "Comedy", color: "#76504c" },
  { title: "Future World", year: 2026, genre: "Sci-Fi", color: "#35446d" }
];

let selectedGenre = "All";

function renderMovies() {
  const query = document.getElementById("search").value
    .trim()
    .toLowerCase();

  const filtered = movies.filter(movie =>
    movie.title.toLowerCase().includes(query) &&
    (selectedGenre === "All" || movie.genre === selectedGenre)
  );

  const list = document.getElementById("movieList");
  list.replaceChildren();

  filtered.forEach(movie => {
    const card = document.createElement("article");
    card.className = "movie";

    const poster = document.createElement("div");
    poster.className = "poster";
    poster.style.background =
      `linear-gradient(145deg, ${movie.color}, #171720)`;
    poster.textContent = movie.title;

    const info = document.createElement("div");
    info.className = "movie-info";

    const title = document.createElement("h3");
    title.textContent = movie.title;

    const details = document.createElement("p");
    details.textContent = `${movie.year} · ${movie.genre}`;

    const link = document.createElement("a");
    link.href =
      "https://www.youtube.com/results?search_query=" +
      encodeURIComponent(movie.title + " official trailer");
    link.target = "_blank";
    link.rel = "noopener noreferrer";
    link.textContent = "Find Trailer ↗";

    info.append(title, details, link);
    card.append(poster, info);
    list.append(card);
  });

  document.getElementById("empty").style.display =
    filtered.length ? "none" : "block";
}

document.getElementById("search").addEventListener("input", renderMovies);

document.querySelectorAll(".category").forEach(button => {
  button.addEventListener("click", () => {
    selectedGenre = button.dataset.genre;

    document.querySelectorAll(".category").forEach(item => {
      item.classList.toggle("active", item === button);
    });

    renderMovies();
  });
});

renderMovies();
</script>

</body>
</html>
