import React, { useState, useEffect, useMemo } from 'react';
import { 
  Play, Home, Compass, Clock, ThumbsUp, ThumbsDown, 
  Upload, Search, Menu, User, MoreVertical, MessageSquare, 
  Share2, Bell, X
} from 'lucide-react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from 'firebase/auth';
import { getFirestore, collection, onSnapshot, doc, setDoc, addDoc, deleteDoc } from 'firebase/firestore';

// --- FİREBASE KURULUMU ---
const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'gittube-app-id';

export default function App() {
  const [user, setUser] = useState(null);
  const [isSidebarOpen, setIsSidebarOpen] = useState(true);
  const [currentRoute, setCurrentRoute] = useState('home'); // 'home' veya 'watch'
  const [activeVideo, setActiveVideo] = useState(null);
  const [isUploadModalOpen, setIsUploadModalOpen] = useState(false);

  // Veritabanı State'leri
  const [videos, setVideos] = useState([]);
  const [views, setViews] = useState([]);
  const [likes, setLikes] = useState([]);
  const [comments, setComments] = useState([]);
  
  const [searchQuery, setSearchQuery] = useState('');

  // --- KURAL 3: KİMLİK DOĞRULAMA (İLK İŞLEM) ---
  useEffect(() => {
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (error) {
        console.error("Giriş hatası:", error);
      }
    };
    initAuth();
    const unsubscribe = onAuthStateChanged(auth, setUser);
    return () => unsubscribe();
  }, []);

  // --- VERİTABANI DİNLEYİCİLERİ ---
  useEffect(() => {
    if (!user) return; // Kullanıcı yoksa veritabanına dokunma

    const collections = ['videos', 'views', 'likes', 'comments'];
    const unsubscribes = [];

    collections.forEach(colName => {
      const colRef = collection(db, 'artifacts', appId, 'public', 'data', colName);
      const unsub = onSnapshot(
        colRef, 
        (snapshot) => {
          const data = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
          if (colName === 'videos') setVideos(data.sort((a,b) => b.createdAt - a.createdAt));
          if (colName === 'views') setViews(data);
          if (colName === 'likes') setLikes(data);
          if (colName === 'comments') setComments(data.sort((a,b) => b.createdAt - a.createdAt));
        },
        (error) => console.error(`${colName} çekilirken hata:`, error)
      );
      unsubscribes.push(unsub);
    });

    return () => unsubscribes.forEach(unsub => unsub());
  }, [user]);

  // --- İZLENME ALGORİTMASI (1 KİŞİ = 1 İZLENME) ---
  useEffect(() => {
    if (user && activeVideo && currentRoute === 'watch') {
      const recordView = async () => {
        // ID olarak videoId_userId kullanıyoruz. 
        // Bu sayede bir kullanıcı bir videoyu defalarca açsa da aynı belge güncellenir, yeni belge oluşmaz.
        const viewId = `${activeVideo.id}_${user.uid}`;
        const viewRef = doc(db, 'artifacts', appId, 'public', 'data', 'views', viewId);
        await setDoc(viewRef, {
          videoId: activeVideo.id,
          userId: user.uid,
          lastViewed: Date.now()
        }, { merge: true });
      };
      recordView();
    }
  }, [activeVideo, user, currentRoute]);


  // Oynatılacak videoyu açma fonksiyonu
  const playVideo = (video) => {
    setActiveVideo(video);
    setCurrentRoute('watch');
    window.scrollTo(0, 0);
  };

  const goHome = () => {
    setCurrentRoute('home');
    setActiveVideo(null);
  };

  // --- HESAPLANMIŞ VERİLER (BELLEKTE FİLTRELEME - KURAL 2) ---
  const filteredVideos = useMemo(() => {
    return videos.filter(v => v.title.toLowerCase().includes(searchQuery.toLowerCase()));
  }, [videos, searchQuery]);

  const getVideoStats = (videoId) => {
    const videoViews = views.filter(v => v.videoId === videoId).length;
    const videoLikes = likes.filter(l => l.videoId === videoId && l.type === 'like').length;
    return { views: videoViews, likes: videoLikes };
  };

  if (!user) {
    return (
      <div className="min-h-screen bg-[#0f0f0f] text-white flex items-center justify-center">
        <div className="animate-pulse flex flex-col items-center">
          <Play size={48} className="text-red-600 mb-4" />
          <h1 className="text-2xl font-bold">GitTube Başlatılıyor...</h1>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-[#0f0f0f] text-white font-sans overflow-x-hidden">
      {/* ÜST BİLGİ (HEADER) */}
      <header className="fixed top-0 left-0 right-0 h-16 bg-[#0f0f0f] flex items-center justify-between px-4 z-50">
        <div className="flex items-center gap-4">
          <button onClick={() => setIsSidebarOpen(!isSidebarOpen)} className="p-2 hover:bg-zinc-800 rounded-full transition">
            <Menu size={24} />
          </button>
          <div className="flex items-center gap-1 cursor-pointer" onClick={goHome}>
            <div className="bg-red-600 p-1 rounded-lg">
              <Play size={20} fill="white" className="text-white" />
            </div>
            <span className="text-xl font-bold tracking-tighter">GitTube</span>
          </div>
        </div>

        <div className="flex-1 max-w-2xl px-12 hidden md:flex items-center">
          <div className="flex w-full">
            <input 
              type="text" 
              placeholder="Ara..." 
              value={searchQuery}
              onChange={(e) => setSearchQuery(e.target.value)}
              className="w-full bg-[#121212] border border-zinc-700 rounded-l-full px-4 py-2 focus:outline-none focus:border-blue-500"
            />
            <button className="bg-[#222222] border border-l-0 border-zinc-700 rounded-r-full px-5 py-2 hover:bg-zinc-800 transition">
              <Search size={20} />
            </button>
          </div>
        </div>

        <div className="flex items-center gap-2 md:gap-4">
          <button 
            onClick={() => setIsUploadModalOpen(true)}
            className="flex items-center gap-2 bg-zinc-800 hover:bg-zinc-700 px-3 py-2 rounded-full transition"
          >
            <Upload size={20} />
            <span className="hidden md:inline text-sm font-medium">Video Yükle</span>
          </button>
          <button className="p-2 hover:bg-zinc-800 rounded-full transition hidden sm:block">
            <Bell size={20} />
          </button>
          <div className="w-8 h-8 bg-purple-600 rounded-full flex items-center justify-center font-bold">
            {user.uid.substring(0,1).toUpperCase()}
          </div>
        </div>
      </header>

      {/* YAN MENÜ (SIDEBAR) & ANA İÇERİK */}
      <div className="flex pt-16 h-[calc(100vh)]">
        {/* Sidebar */}
        <aside className={`${isSidebarOpen ? 'w-64' : 'w-0 sm:w-20'} transition-all duration-300 bg-[#0f0f0f] fixed h-full z-40 overflow-hidden flex flex-col`}>
          <div className="p-2 flex-1">
            <SidebarItem icon={<Home />} label="Ana Sayfa" isOpen={isSidebarOpen} active={currentRoute === 'home'} onClick={goHome} />
            <SidebarItem icon={<Compass />} label="Keşfet" isOpen={isSidebarOpen} />
            <SidebarItem icon={<Clock />} label="Geçmiş" isOpen={isSidebarOpen} />
            <div className="my-3 border-b border-zinc-800" />
            {isSidebarOpen && <h3 className="px-3 text-zinc-400 font-semibold mb-2 mt-4">Siz</h3>}
            <SidebarItem icon={<User />} label="Kanalınız" isOpen={isSidebarOpen} />
            <SidebarItem icon={<ThumbsUp />} label="Beğendiğim Videolar" isOpen={isSidebarOpen} />
          </div>
        </aside>

        {/* Ana İçerik Alanı */}
        <main className={`flex-1 overflow-y-auto ${isSidebarOpen ? 'ml-64' : 'ml-0 sm:ml-20'} transition-all duration-300 p-4 sm:p-6`}>
          {currentRoute === 'home' && (
            <div>
              {/* Kategori Etiketleri */}
              <div className="flex gap-3 mb-6 overflow-x-auto pb-2 scrollbar-hide">
                {['Tümü', 'Oyun', 'Müzik', 'Canlı', 'Yazılım', 'Haberler', 'Podcastler'].map(tag => (
                  <button key={tag} className="bg-zinc-800 hover:bg-zinc-700 px-4 py-1.5 rounded-lg whitespace-nowrap text-sm font-medium transition">
                    {tag}
                  </button>
                ))}
              </div>

              {/* Video Grid */}
              <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 sm:gap-6">
                {filteredVideos.length === 0 ? (
                  <div className="col-span-full text-center py-20 text-zinc-500">
                    <Play size={48} className="mx-auto mb-4 opacity-20" />
                    <p className="text-xl">Henüz video bulunmuyor.</p>
                    <p className="text-sm mt-2">İlk videoyu yükleyen sen ol!</p>
                  </div>
                ) : (
                  filteredVideos.map(video => (
                    <VideoCard 
                      key={video.id} 
                      video={video} 
                      stats={getVideoStats(video.id)} 
                      onClick={() => playVideo(video)} 
                    />
                  ))
                )}
              </div>
            </div>
          )}

          {currentRoute === 'watch' && activeVideo && (
            <WatchPage 
              video={activeVideo}
              videos={videos}
              views={views}
              likes={likes}
              comments={comments}
              user={user}
              appId={appId}
              db={db}
              playVideo={playVideo}
            />
          )}
        </main>
      </div>

      {/* VİDEO YÜKLEME MODALI */}
      {isUploadModalOpen && (
        <UploadModal 
          onClose={() => setIsUploadModalOpen(false)} 
          user={user}
          db={db}
          appId={appId}
        />
      )}
    </div>
  );
}

// --- ALT BİLEŞENLER ---

function SidebarItem({ icon, label, isOpen, active, onClick }) {
  return (
    <div 
      onClick={onClick}
      className={`flex items-center p-3 mb-1 rounded-xl cursor-pointer transition ${active ? 'bg-zinc-800' : 'hover:bg-zinc-800'} ${!isOpen ? 'justify-center' : ''}`}
      title={!isOpen ? label : ''}
    >
      <div className="min-w-[24px]">{icon}</div>
      {isOpen && <span className="ml-4 truncate text-sm">{label}</span>}
    </div>
  );
}

function VideoCard({ video, stats, onClick }) {
  // Basit tarih formatlayıcı
  const timeAgo = (timestamp) => {
    const seconds = Math.floor((new Date() - timestamp) / 1000);
    let interval = seconds / 31536000;
    if (interval > 1) return Math.floor(interval) + " yıl önce";
    interval = seconds / 2592000;
    if (interval > 1) return Math.floor(interval) + " ay önce";
    interval = seconds / 86400;
    if (interval > 1) return Math.floor(interval) + " gün önce";
    interval = seconds / 3600;
    if (interval > 1) return Math.floor(interval) + " saat önce";
    interval = seconds / 60;
    if (interval > 1) return Math.floor(interval) + " dakika önce";
    return Math.floor(seconds) + " saniye önce";
  };

  return (
    <div className="flex flex-col gap-3 cursor-pointer group" onClick={onClick}>
      {/* Thumbnail */}
      <div className="relative w-full aspect-video bg-zinc-800 rounded-xl overflow-hidden">
        <img 
          src={video.thumbnail || "https://images.unsplash.com/photo-1611162617474-5b21e879e113?ixlib=rb-4.0.3&auto=format&fit=crop&w=800&q=80"} 
          alt={video.title}
          className="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"
          onError={(e) => { e.target.src = "https://images.unsplash.com/photo-1611162617474-5b21e879e113?ixlib=rb-4.0.3&auto=format&fit=crop&w=800&q=80" }}
        />
        <div className="absolute bottom-1 right-1 bg-black bg-opacity-80 text-xs px-1.5 py-0.5 rounded">
          {Math.floor(Math.random() * 10) + 1}:{Math.floor(Math.random() * 50) + 10}
        </div>
      </div>
      
      {/* Detaylar */}
      <div className="flex gap-3 pr-6">
        <div className="w-9 h-9 rounded-full bg-blue-600 flex-shrink-0 flex items-center justify-center font-bold text-sm mt-0.5">
          {video.uploaderName?.substring(0,1).toUpperCase() || "A"}
        </div>
        <div className="flex flex-col overflow-hidden">
          <h3 className="text-base font-semibold leading-tight line-clamp-2 text-zinc-100 group-hover:text-blue-400 transition">
            {video.title}
          </h3>
          <span className="text-sm text-zinc-400 mt-1">{video.uploaderName || "Anonim Kullanıcı"}</span>
          <div className="flex text-sm text-zinc-400 items-center gap-1">
            <span>{stats.views} görüntülenme</span>
            <span className="text-[10px]">•</span>
            <span>{timeAgo(video.createdAt)}</span>
          </div>
        </div>
      </div>
    </div>
  );
}

// OYNATMA SAYFASI (WATCH PAGE)
function WatchPage({ video, videos, views, likes, comments, user, appId, db, playVideo }) {
  const [commentText, setCommentText] = useState('');

  const videoViews = views.filter(v => v.videoId === video.id).length;
  const userLikeStatus = likes.find(l => l.videoId === video.id && l.userId === user.uid)?.type; // 'like', 'dislike' veya undefined
  const likeCount = likes.filter(l => l.videoId === video.id && l.type === 'like').length;
  
  const videoComments = comments.filter(c => c.videoId === video.id);
  const suggestedVideos = videos.filter(v => v.id !== video.id).slice(0, 10);

  // Beğeni Sistemi
  const handleLikeAction = async (type) => {
    const likeId = `${video.id}_${user.uid}`;
    const likeRef = doc(db, 'artifacts', appId, 'public', 'data', 'likes', likeId);
    
    if (userLikeStatus === type) {
      // Zaten beğenmişse ve tekrar basarsa beğeniyi kaldır
      await deleteDoc(likeRef);
    } else {
      // Yeni beğeni veya disliketan like'a geçiş
      await setDoc(likeRef, {
        videoId: video.id,
        userId: user.uid,
        type: type,
        timestamp: Date.now()
      });
    }
  };

  // Yorum Gönderme
  const submitComment = async (e) => {
    e.preventDefault();
    if (!commentText.trim()) return;

    const commentsRef = collection(db, 'artifacts', appId, 'public', 'data', 'comments');
    await addDoc(commentsRef, {
      videoId: video.id,
      userId: user.uid,
      userName: `Kullanıcı_${user.uid.substring(0, 4)}`,
      text: commentText,
      createdAt: Date.now()
    });
    setCommentText('');
  };

  return (
    <div className="flex flex-col lg:flex-row gap-6 max-w-[1600px] mx-auto">
      {/* Sol Taraf: Video ve Detaylar */}
      <div className="flex-1 lg:w-2/3 xl:w-3/4">
        {/* Video Oynatıcı */}
        <div className="w-full aspect-video bg-black rounded-xl overflow-hidden mb-4 shadow-lg shadow-black/50">
          <video 
            src={video.videoUrl} 
            controls 
            autoPlay
            className="w-full h-full"
            poster={video.thumbnail}
            onError={(e) => console.log("Video yüklenemedi, örnek video eklenebilir.")}
          >
            Tarayıcınız video etiketini desteklemiyor.
          </video>
        </div>

        {/* Video Başlığı ve Bilgiler */}
        <h1 className="text-xl sm:text-2xl font-bold mb-3">{video.title}</h1>
        
        <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-4">
          {/* Kanal Bilgisi */}
          <div className="flex items-center gap-3">
            <div className="w-10 h-10 rounded-full bg-purple-600 flex items-center justify-center font-bold text-lg">
              {video.uploaderName?.substring(0,1).toUpperCase() || "A"}
            </div>
            <div>
              <h3 className="font-bold text-base">{video.uploaderName || "Anonim Kanal"}</h3>
              <p className="text-xs text-zinc-400">1.2B abone</p>
            </div>
            <button className="ml-4 bg-white text-black font-semibold px-4 py-2 rounded-full hover:bg-zinc-200 transition">
              Abone Ol
            </button>
          </div>

          {/* Aksiyon Butonları (Like vs) */}
          <div className="flex items-center gap-2 w-full sm:w-auto overflow-x-auto pb-2 sm:pb-0 scrollbar-hide">
            <div className="flex bg-zinc-800 rounded-full">
              <button 
                onClick={() => handleLikeAction('like')}
                className={`flex items-center gap-2 px-4 py-2 rounded-l-full hover:bg-zinc-700 transition ${userLikeStatus === 'like' ? 'text-blue-400' : ''}`}
              >
                <ThumbsUp size={20} className={userLikeStatus === 'like' ? "fill-blue-400" : ""} />
                <span className="font-medium">{likeCount || 'Beğen'}</span>
              </button>
              <div className="w-[1px] bg-zinc-600 my-2"></div>
              <button 
                onClick={() => handleLikeAction('dislike')}
                className={`flex items-center px-4 py-2 rounded-r-full hover:bg-zinc-700 transition ${userLikeStatus === 'dislike' ? 'text-red-400' : ''}`}
              >
                <ThumbsDown size={20} className={userLikeStatus === 'dislike' ? "fill-red-400" : ""} />
              </button>
            </div>
            
            <button className="flex items-center gap-2 bg-zinc-800 hover:bg-zinc-700 px-4 py-2 rounded-full transition whitespace-nowrap">
              <Share2 size={20} />
              <span className="font-medium hidden sm:inline">Paylaş</span>
            </button>
            
            <button className="bg-zinc-800 hover:bg-zinc-700 p-2.5 rounded-full transition">
              <MoreVertical size={20} />
            </button>
          </div>
        </div>

        {/* Açıklama Kutusu */}
        <div className="bg-zinc-800/80 rounded-xl p-4 mb-6 hover:bg-zinc-800 transition cursor-pointer">
          <div className="flex gap-2 font-bold text-sm mb-1">
            <span>{videoViews} görüntülenme</span>
            <span>•</span>
            <span>{new Date(video.createdAt).toLocaleDateString('tr-TR')}</span>
          </div>
          <p className="text-sm whitespace-pre-wrap">{video.description || "Bu video için açıklama girilmemiş."}</p>
        </div>

        {/* YORUMLAR BÖLÜMÜ */}
        <div>
          <h3 className="text-xl font-bold mb-6">{videoComments.length} Yorum</h3>
          
          {/* Yorum Ekleme */}
          <div className="flex gap-4 mb-8">
            <div className="w-10 h-10 rounded-full bg-purple-600 flex-shrink-0 flex items-center justify-center font-bold">
              {user.uid.substring(0,1).toUpperCase()}
            </div>
            <form onSubmit={submitComment} className="flex-1 flex flex-col items-end">
              <input 
                type="text" 
                value={commentText}
                onChange={(e) => setCommentText(e.target.value)}
                placeholder="Yorum ekleyin..." 
                className="w-full bg-transparent border-b border-zinc-600 focus:border-white outline-none py-1 mb-2 text-sm transition-colors"
              />
              <div className="flex gap-2 mt-2">
                <button 
                  type="button" 
                  onClick={() => setCommentText('')}
                  className="px-4 py-2 text-sm font-medium hover:bg-zinc-800 rounded-full transition"
                >
                  İptal
                </button>
                <button 
                  type="submit"
                  disabled={!commentText.trim()}
                  className="px-4 py-2 bg-blue-600 hover:bg-blue-500 disabled:bg-zinc-800 disabled:text-zinc-500 text-sm font-medium rounded-full transition"
                >
                  Yorum Yap
                </button>
              </div>
            </form>
          </div>

          {/* Yorum Listesi */}
          <div className="flex flex-col gap-6">
            {videoComments.map(comment => (
              <div key={comment.id} className="flex gap-4">
                <div className="w-10 h-10 rounded-full bg-zinc-700 flex-shrink-0 flex items-center justify-center font-bold text-sm">
                  {comment.userName.substring(0,1).toUpperCase()}
                </div>
                <div>
                  <div className="flex gap-2 items-center mb-1">
                    <span className="font-bold text-[13px]">@{comment.userName}</span>
                    <span className="text-xs text-zinc-400">
                      {new Date(comment.createdAt).toLocaleDateString()}
                    </span>
                  </div>
                  <p className="text-sm">{comment.text}</p>
                  <div className="flex items-center gap-4 mt-2">
                    <button className="hover:text-blue-400 transition"><ThumbsUp size={14} /></button>
                    <button className="hover:text-red-400 transition"><ThumbsDown size={14} /></button>
                    <button className="text-xs font-semibold hover:bg-zinc-800 px-3 py-1 rounded-full transition">Yanıtla</button>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>

      {/* Sağ Taraf: Önerilen Videolar */}
      <div className="lg:w-1/3 xl:w-1/4 flex flex-col gap-3">
        <h3 className="font-bold text-lg mb-2">Sıradaki Videolar</h3>
        {suggestedVideos.length === 0 && (
          <p className="text-zinc-500 text-sm">Başka video bulunamadı.</p>
        )}
        {suggestedVideos.map(v => (
          <div key={v.id} className="flex gap-2 cursor-pointer group" onClick={() => playVideo(v)}>
            <div className="relative w-40 flex-shrink-0 aspect-video bg-zinc-800 rounded-lg overflow-hidden">
              <img 
                src={v.thumbnail} 
                alt={v.title}
                className="w-full h-full object-cover group-hover:scale-105 transition-transform"
                onError={(e) => { e.target.src = "https://images.unsplash.com/photo-1611162617474-5b21e879e113?ixlib=rb-4.0.3&auto=format&fit=crop&w=400&q=80" }}
              />
            </div>
            <div className="flex flex-col py-0.5">
              <h4 className="text-sm font-semibold line-clamp-2 leading-tight group-hover:text-blue-400">{v.title}</h4>
              <span className="text-xs text-zinc-400 mt-1">{v.uploaderName}</span>
              <span className="text-xs text-zinc-400">
                {views.filter(view => view.videoId === v.id).length} izlenme
              </span>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}

// VİDEO YÜKLEME MODALI
function UploadModal({ onClose, user, db, appId }) {
  const [title, setTitle] = useState('');
  const [description, setDescription] = useState('');
  // Gerçek dosya yüklemek yerine, platformun çalışması için bir URL girmesini istiyoruz
  const [videoUrl, setVideoUrl] = useState('https://www.w3schools.com/html/mov_bbb.mp4'); 
  const [thumbnailUrl, setThumbnailUrl] = useState('https://images.unsplash.com/photo-1611162617474-5b21e879e113?ixlib=rb-4.0.3&auto=format&fit=crop&w=800&q=80');
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleUpload = async (e) => {
    e.preventDefault();
    if (!title.trim() || !videoUrl.trim()) return;
    
    setIsSubmitting(true);
    try {
      const videosRef = collection(db, 'artifacts', appId, 'public', 'data', 'videos');
      await addDoc(videosRef, {
        title,
        description,
        videoUrl,
        thumbnail: thumbnailUrl,
        uploaderId: user.uid,
        uploaderName: `Yaratıcı_${user.uid.substring(0, 4)}`,
        createdAt: Date.now()
      });
      onClose();
    } catch (error) {
      console.error("Yükleme hatası:", error);
      alert("Yüklenirken bir hata oluştu.");
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <div className="fixed inset-0 bg-black/80 z-[100] flex items-center justify-center p-4">
      <div className="bg-[#1f1f1f] rounded-2xl w-full max-w-2xl max-h-[90vh] overflow-y-auto flex flex-col shadow-2xl">
        <div className="flex justify-between items-center p-6 border-b border-zinc-800">
          <h2 className="text-xl font-bold">Video Yükle</h2>
          <button onClick={onClose} className="p-2 hover:bg-zinc-700 rounded-full transition">
            <X size={24} />
          </button>
        </div>
        
        <form onSubmit={handleUpload} className="p-6 flex flex-col gap-6">
          
          <div className="flex flex-col gap-2">
            <label className="text-sm font-semibold text-zinc-300">Video Başlığı *</label>
            <input 
              type="text" 
              value={title}
              onChange={(e) => setTitle(e.target.value)}
              placeholder="Videonuzu anlatan dikkat çekici bir başlık ekleyin"
              required
              className="bg-zinc-900 border border-zinc-700 rounded-lg px-4 py-3 focus:outline-none focus:border-blue-500 transition"
            />
          </div>

          <div className="flex flex-col gap-2">
            <label className="text-sm font-semibold text-zinc-300">Açıklama</label>
            <textarea 
              value={description}
              onChange={(e) => setDescription(e.target.value)}
              placeholder="İzleyicilerinize videonuzdan bahsedin"
              rows={4}
              className="bg-zinc-900 border border-zinc-700 rounded-lg px-4 py-3 focus:outline-none focus:border-blue-500 transition resize-none"
            />
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div className="flex flex-col gap-2">
              <label className="text-sm font-semibold text-zinc-300">Video URL (Geliştirici Modu)</label>
              <input 
                type="url" 
                value={videoUrl}
                onChange={(e) => setVideoUrl(e.target.value)}
                placeholder="https://..."
                required
                className="bg-zinc-900 border border-zinc-700 rounded-lg px-4 py-3 focus:outline-none focus:border-blue-500 transition text-sm text-blue-400"
              />
              <span className="text-xs text-zinc-500">Not: Sistem şu an video dosyasını yüklemek yerine URL'den oynatacak şekilde (simülasyon) ayarlanmıştır.</span>
            </div>
            
            <div className="flex flex-col gap-2">
              <label className="text-sm font-semibold text-zinc-300">Kapak Fotoğrafı URL (Thumbnail)</label>
              <input 
                type="url" 
                value={thumbnailUrl}
                onChange={(e) => setThumbnailUrl(e.target.value)}
                placeholder="Resim linki ekleyin..."
                className="bg-zinc-900 border border-zinc-700 rounded-lg px-4 py-3 focus:outline-none focus:border-blue-500 transition text-sm"
              />
            </div>
          </div>

          <div className="border-t border-zinc-800 pt-6 flex justify-end gap-3 mt-2">
            <button 
              type="button" 
              onClick={onClose}
              className="px-6 py-2.5 rounded-full hover:bg-zinc-800 transition font-medium"
            >
              İptal
            </button>
            <button 
              type="submit"
              disabled={isSubmitting || !title.trim()}
              className="bg-blue-600 hover:bg-blue-500 disabled:bg-zinc-800 disabled:text-zinc-500 px-6 py-2.5 rounded-full font-medium transition flex items-center gap-2"
            >
              {isSubmitting ? (
                <>Yükleniyor...</>
              ) : (
                <>
                  <Upload size={18} />
                  Videoyu Yayınla
                </>
              )}
            </button>
          </div>
        </form>
      </div>
    </div>
  );
}
