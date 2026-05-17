# Besanese_Basket
Basket
export default function DragonHoopsSite() { const [posts, setPosts] = React.useState([]); const [caption, setCaption] = React.useState(""); const [commentInputs, setCommentInputs] = React.useState({});

const handleUpload = (e) => { const files = Array.from(e.target.files || []);

const newPosts = files.map((file) => ({
  id: crypto.randomUUID(),
  file,
  url: URL.createObjectURL(file),
  type: file.type.startsWith("video") ? "video" : "image",
  caption,
  comments: [],
  createdAt: new Date().toLocaleString(),
}));

setPosts((prev) => [...newPosts, ...prev]);
setCaption("");

};

const addComment = (id) => { if (!commentInputs[id]) return;

setPosts((prev) =>
  prev.map((post) =>
    post.id === id
      ? {
          ...post,
          comments: [...post.comments, commentInputs[id]],
        }
      : post
  )
);

setCommentInputs((prev) => ({
  ...prev,
  [id]: "",
}));

};

const downloadMedia = async (url, filename) => { const response = await fetch(url); const blob = await response.blob(); const blobUrl = URL.createObjectURL(blob);

const a = document.createElement("a");
a.href = blobUrl;
a.download = filename;
document.body.appendChild(a);
a.click();
document.body.removeChild(a);

URL.revokeObjectURL(blobUrl);

};

return ( <div className="min-h-screen bg-black text-white overflow-hidden"> <section className="relative border-b border-red-900 bg-gradient-to-b from-black via-red-950 to-black"> <div className="absolute inset-0 opacity-20 bg-[radial-gradient(circle_at_center,red,transparent_70%)]" />

<div className="relative max-w-7xl mx-auto px-6 py-16 flex flex-col lg:flex-row items-center justify-between gap-12">
      <div className="flex-1 space-y-6">
        <div className="flex items-center gap-5 flex-wrap">
          <img
            src="/mnt/data/1000006443.png"
            alt="Dragon Basket Logo"
            className="w-28 h-28 object-contain drop-shadow-[0_0_25px_rgba(255,0,0,0.7)]"
          />

          <div>
            <h1 className="text-5xl md:text-7xl font-black tracking-tight text-red-500">
              DRAGON HOOPS
            </h1>

            <p className="text-zinc-300 text-lg mt-3 max-w-2xl">
              La piattaforma basket dove la community può caricare
              highlights, schiacciate, crossover, allenamenti e momenti
              epici.
            </p>
          </div>
        </div>

        <div className="flex flex-wrap gap-4 pt-4">
          <div className="px-5 py-3 rounded-2xl bg-red-600/20 border border-red-500 text-red-300 backdrop-blur-md">
            Upload Foto
          </div>

          <div className="px-5 py-3 rounded-2xl bg-red-600/20 border border-red-500 text-red-300 backdrop-blur-md">
            Upload Video
          </div>

          <div className="px-5 py-3 rounded-2xl bg-red-600/20 border border-red-500 text-red-300 backdrop-blur-md">
            Commenti Community
          </div>

          <div className="px-5 py-3 rounded-2xl bg-red-600/20 border border-red-500 text-red-300 backdrop-blur-md">
            Download Contenuti
          </div>
        </div>
      </div>

      <div className="flex-1 w-full max-w-xl">
        <div className="bg-zinc-950 border border-red-900 rounded-3xl p-8 shadow-2xl shadow-red-900/30 backdrop-blur-xl">
          <h2 className="text-3xl font-bold text-red-500 mb-6">
            Carica il tuo contenuto
          </h2>

          <div className="space-y-5">
            <textarea
              value={caption}
              onChange={(e) => setCaption(e.target.value)}
              placeholder="Scrivi una descrizione o didascalia..."
              className="w-full h-32 bg-black border border-red-800 rounded-2xl p-4 text-white outline-none focus:border-red-500"
            />

            <label className="flex items-center justify-center border-2 border-dashed border-red-700 rounded-2xl p-8 cursor-pointer hover:border-red-500 transition">
              <div className="text-center">
                <p className="text-xl font-semibold text-red-400">
                  Seleziona foto o video
                </p>

                <p className="text-zinc-500 text-sm mt-2">
                  Supporto immagini e video
                </p>
              </div>

              <input
                type="file"
                multiple
                accept="image/*,video/*"
                className="hidden"
                onChange={handleUpload}
              />
            </label>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section className="max-w-7xl mx-auto px-6 py-16">
    <div className="mb-12">
      <h2 className="text-4xl font-black text-red-500">
        Community Feed
      </h2>

      <p className="text-zinc-400 mt-3">
        Guarda, commenta e scarica i migliori contenuti basket.
      </p>
    </div>

    {posts.length === 0 ? (
      <div className="border border-red-900 rounded-3xl p-20 text-center bg-zinc-950">
        <p className="text-3xl font-bold text-red-500">
          Nessun contenuto caricato
        </p>

        <p className="text-zinc-500 mt-3">
          Inizia caricando foto o video della community.
        </p>
      </div>
    ) : (
      <div className="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-8">
        {posts.map((post) => (
          <div
            key={post.id}
            className="bg-zinc-950 border border-red-900 rounded-3xl overflow-hidden shadow-xl hover:shadow-red-900/30 transition"
          >
            <div className="aspect-square bg-black overflow-hidden">
              {post.type === "image" ? (
                <img
                  src={post.url}
                  alt="Uploaded"
                  className="w-full h-full object-cover"
                />
              ) : (
                <video
                  src={post.url}
                  controls
                  className="w-full h-full object-cover"
                />
              )}
            </div>

            <div className="p-5 space-y-5">
              <div>
                <p className="text-zinc-300">
                  {post.caption || "Nessuna descrizione"}
                </p>

                <span className="text-xs text-zinc-500 block mt-3">
                  {post.createdAt}
                </span>
              </div>

              <button
                onClick={() =>
                  downloadMedia(post.url, post.file.name)
                }
                className="w-full py-3 rounded-2xl bg-red-600 hover:bg-red-500 transition font-bold"
              >
                Scarica contenuto
              </button>

              <div className="space-y-3">
                <div className="flex gap-2">
                  <input
                    type="text"
                    placeholder="Scrivi un commento..."
                    value={commentInputs[post.id] || ""}
                    onChange={(e) =>
                      setCommentInputs((prev) => ({
                        ...prev,
                        [post.id]: e.target.value,
                      }))
                    }
                    className="flex-1 bg-black border border-red-800 rounded-xl px-4 py-3 outline-none focus:border-red-500"
                  />

                  <button
                    onClick={() => addComment(post.id)}
                    className="px-5 rounded-xl bg-red-600 hover:bg-red-500 font-semibold"
                  >
                    Invia
                  </button>
                </div>

                <div className="space-y-2 max-h-40 overflow-y-auto">
                  {post.comments.length === 0 ? (
                    <p className="text-sm text-zinc-500">
                      Nessun commento.
                    </p>
                  ) : (
                    post.comments.map((comment, index) => (
                      <div
                        key={index}
                        className="bg-black border border-zinc-800 rounded-xl px-4 py-3 text-sm text-zinc-300"
                      >
                        {comment}
                      </div>
                    ))
                  )}
                </div>
              </div>
            </div>
          </div>
        ))}
      </div>
    )}
  </section>

  <footer className="border-t border-red-950 py-10 text-center text-zinc-500 bg-black">
    <p>© 2026 Dragon Hoops — Basket Community Platform</p>
  </footer>
</div>

); }
