<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DiziFlix</title>
  <script src="https://cdn.jsdelivr.net/npm/react@18.2.0/umd/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/react-dom@18.2.0/umd/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/babel-standalone@7.22.9/babel.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-900 text-white">
  <div id="root"></div>

  <script type="text/babel">
    const { useState } = React;

    const content = [
      { id: 1, title: "The Dark Knight", genre: "Aksiyon", category: "Film", image: "https://via.placeholder.com/200x300?text=Dark+Knight", videoUrl: "https://www.w3schools.com/html/mov_bbb.mp4" },
      { id: 2, title: "Stranger Things", genre: "Bilim Kurgu", category: "Dizi", image: "https://via.placeholder.com/200x300?text=Stranger+Things", videoUrl: "https://www.w3schools.com/html/mov_bbb.mp4" },
    ];

    const MovieCard = ({ item, onPlay }) => (
      <div className="bg-gray-800 rounded-lg p-4">
        <img src={item.image} alt={item.title} className="w-full h-64 object-cover rounded" />
        <h3 className="text-lg font-semibold mt-2">{item.title}</h3>
        <p className="text-sm text-gray-400">{item.genre} • {item.category}</p>
        <button
          onClick={() => onPlay(item)}
          className="mt-2 bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded"
        >
          Oynat
        </button>
      </div>
    );

    const VideoPlayer = ({ item, onClose }) => (
      <div className="fixed inset-0 bg-black bg-opacity-90 flex items-center justify-center z-50">
        <div className="relative w-full max-w-3xl">
          <button
            onClick={onClose}
            className="absolute top-2 right-2 bg-red-600 text-white rounded-full p-2"
          >
            X
          </button>
          <video controls className="w-full rounded" autoPlay>
            <source src={item.videoUrl} type="video/mp4" />
            Tarayıcınız video öğesini desteklemiyor.
          </video>
        </div>
      </div>
    );

    const App = () => {
      const [searchTerm, setSearchTerm] = useState("");
      const [playingItem, setPlayingItem] = useState(null);

      const filteredContent = content.filter(item =>
        item.title.toLowerCase().includes(searchTerm.toLowerCase())
      );

      return (
        <div>
          <header className="bg-gray-800 p-4">
            <div className="container mx-auto flex justify-between items-center">
              <h1 className="text-2xl font-bold">DiziFlix</h1>
              <input
                type="text"
                placeholder="Ara..."
                className="bg-gray-700 text-white px-4 py-2 rounded"
                value={searchTerm}
                onChange={(e) => setSearchTerm(e.target.value)}
              />
            </div>
          </header>

          <main className="container mx-auto p-4">
            <h2 className="text-2xl font-semibold mb-4">Filmler ve Diziler</h2>
            <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
              {filteredContent.length > 0 ? (
                filteredContent.map(item => (
                  <MovieCard key={item.id} item={item} onPlay={setPlayingItem} />
                ))
              ) : (
                <p className="text-gray-400">Sonuç bulunamadı.</p>
              )}
            </div>
          </main>

          {playingItem && (
            <VideoPlayer item={playingItem} onClose={() => setPlayingItem(null)} />
          )}
        </div>
      );
    };

    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<App />);
  </script>
</body>
</html>
