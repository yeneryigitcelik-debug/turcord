# SohbetApp — Turkiye'nin Discord'u

Turkiye pazarina ozel, gercek zamanli iletisim platformu. Discord'un birebir klonu olarak tasarlanmis, Turkce oncelikli bir topluluk uygulamasi.

## Tech Stack

| Katman | Teknoloji |
|--------|-----------|
| Frontend | Next.js 14 (App Router), TypeScript, Tailwind CSS |
| Backend/DB | Supabase (PostgreSQL, Auth, Realtime, Storage) |
| Gercek Zamanli | Supabase Realtime (presence + broadcast) + LiveKit (sesli/goruntulu) |
| State Management | Zustand |
| UI | Radix UI primitives + Discord-tarzi custom bilesenler |
| Deployment | Docker (Hetzner VPS) |

## Proje Yapisi

```
src/
├── app/                    # Next.js App Router
│   ├── (auth)/            # Login, Register, Forgot Password
│   ├── (main)/            # Ana uygulama layout
│   │   ├── channels/      # Sunucu kanallari
│   │   ├── @me/           # DM'ler ve arkadas listesi
│   │   └── discover/      # Sunucu kesfet
│   └── invite/[code]/     # Davet linki sayfasi
├── components/
│   ├── ui/                # Temel UI bilesenleri (Button, Modal, Tooltip vs.)
│   ├── chat/              # Mesaj, MessageInput, EmojiPicker, ReactionBar
│   ├── server/            # ServerSidebar, ChannelList, MemberList
│   ├── voice/             # VoiceChannel, VoiceControls, ScreenShare
│   ├── modals/            # CreateServer, InviteModal, Settings vs.
│   └── navigation/        # ServerList (sol sidebar), TopNavbar
├── lib/
│   ├── supabase/          # Client, server, middleware helpers
│   ├── store/             # Zustand store'lari
│   ├── hooks/             # Custom hooks
│   ├── utils/             # Yardimci fonksiyonlar
│   └── types/             # TypeScript type tanimlari
├── styles/                # Global CSS, Discord tema degiskenleri
└── public/                # Statik dosyalar
```

## Tema Renkleri

| Degisken | Renk | Kullanim |
|----------|------|----------|
| `bg-primary` | `#313338` | Ana arka plan |
| `bg-secondary` | `#2b2d31` | Sidebar |
| `bg-tertiary` | `#1e1f22` | Sunucu listesi |
| `bg-accent` | `#404249` | Hover |
| `text-primary` | `#f2f3f5` | Ana metin |
| `text-secondary` | `#b5bac1` | Ikincil metin |
| `text-muted` | `#949ba4` | Soluk metin |
| `brand-color` | `#5865f2` | Discord moru |
| `danger` | `#da373c` | Tehlike/silme |
| `success` | `#23a559` | Basari |

## Gelistirme Fazlari

### Phase 0 — Proje Kurulumu ve Temel Altyapi
- Proje olusturma ve bagimliliklarin kurulumu
- Supabase client/server helper konfigurasyonu
- Temel layout yapisi (Discord'un 3-panel layout'u: sol sidebar, kanal listesi, ana icerik)
- Tailwind karanlik tema konfigurasyonu

### Phase 1 — Authentication Sistemi
- Email/Password authentication
- OAuth providers (Google, GitHub)
- Email dogrulama akisi
- `profiles` tablosu ve RLS politikalari
- Login, Register, Forgot Password sayfalari
- Discord tarzi karanlik tema UI
- Form validation (zod + react-hook-form)
- Auth middleware ile korumali route'lar

### Phase 2 — Sunucu (Guild) Sistemi
- `servers`, `categories`, `channels`, `members`, `roles`, `member_roles` tablolari
- RLS politikalari
- Sunucu olustur/katil/ayril fonksiyonlari
- Sol kenar cubugu (ServerList) — yuvarlak ikon, hover animasyonu, pill indicator
- Sunucu olusturma modali
- Kanal listesi sidebar'i (kategoriler, metin/ses kanallari)
- Alt kullanici paneli (avatar, mikrofon/kulaklik kontrolleri, ayarlar)

### Phase 3 — Mesajlasma Sistemi (Gercek Zamanli Chat)
- `messages`, `reactions`, `mentions`, `read_states` tablolari
- Performans indexleri
- Supabase Realtime kurulumu (channel bazli subscription, presence)
- Mesaj gonderme/alma, duzenleme, silme
- Reply, reaksiyon, @mention, dosya yukleme, sabitleme
- "X yaziyor..." gostergesi
- Sonsuz scroll (cursor-based pagination)
- Markdown destegi, link onizleme, kod blogu syntax highlighting
- Emoji picker, GIF butonu, sticker butonu
- Virtualized list (react-virtual)
- Optimistic updates

### Phase 4 — Direkt Mesaj (DM) Sistemi
- `dm_channels`, `dm_participants`, `friendships` tablolari
- @me sayfasi (DM listesi, arkadas listesi)
- Arkadas ekleme/silme/engelleme
- 1-1 ve grup DM (max 10 kisi)
- DM sohbet alani + kullanici profil paneli

### Phase 5 — Ses ve Goruntulu Gorusme
- LiveKit server (Docker ile self-hosted)
- Token olusturma Edge Function
- `voice_states` tablosu
- Ses kanallari (otomatik baglanma, konusan kisi gostergesi)
- Mikrofon/kulaklik/video kontrolleri
- Ekran paylasimi
- DM sesli/goruntulu arama
- PiP ve tam ekran modu

### Phase 6 — Bildirim ve Presence Sistemi
- Cevrimici durum takibi (online, bosta, rahatsiz etmeyin, gorunmez, cevrimdisi)
- Ozel durum mesaji
- `notifications` tablosu
- Mention, DM, arkadaslik istegi, sunucu davet bildirimleri
- Web Push API (Service Worker)
- Bildirim rozetleri (sunucu ikonlari, kanal listesi, DM)

### Phase 7 — Sunucu Ayarlari ve Moderasyon
- Sunucu ayarlar paneli (genel, roller, emoji, moderasyon)
- Bitwise permission sistemi
- `audit_logs`, `bans`, `channel_permission_overrides` tablolari
- Denetim kaydi
- Kick, ban, timeout, mesaj silme
- Davet yonetimi
- Rol olusturma/duzenleme (renk, izin matrisi, drag & drop siralama)

### Phase 8 — Kullanici Ayarlari
- Hesap yonetimi (avatar, banner, kullanici adi, email, sifre)
- Profil onizleme
- Gizlilik ve guvenlik (2FA, DM izinleri)
- Bildirim ayarlari
- Gorunum (tema, font boyutu, mesaj goruntulemesi)
- Ses ve video cihaz ayarlari
- Dil destegi (Turkce/English — next-intl)

### Phase 9 — Arama ve Kesfet
- Full-text search (PostgreSQL tsvector, Turkce dil destegi)
- Arama filtreleri (from, mentions, has, before, after, in)
- Sunucu kesfet sayfasi (/discover)
- `server_discovery` tablosu
- Kategori filtreleri, populer sunucular, arama
- Sunucu kartlari (ikon, isim, aciklama, uye sayisi)

### Phase 10 — Dosya Yonetimi ve Medya
- Supabase Storage bucket'lari (avatars, banners, server-icons, attachments, emojis)
- Drag & Drop yukleme, clipboard paste
- Coklu dosya yukleme, progress bar
- Resim sikistirma (client-side)
- Inline medya goruntuleme (resim lightbox, video player, audio player)
- Storage RLS politikalari

### Phase 11 — Thread ve Forum Kanallari
- `threads` tablosu
- Thread olusturma/arsivleme
- Thread paneli (sag panel)
- Forum kanali tipi
- Post olusturma (baslik + icerik + etiketler)
- Etiket sistemi, siralama, grid/liste gorunumu

### Phase 12 — Performans, PWA ve Son Rotuslar
- Next.js optimizasyonlari (dynamic imports, image optimization, server components)
- Database optimizasyonlari (connection pooling, materialized views)
- Client-side cache (React Query / SWR, Zustand persistence)
- PWA kurulumu (Service Worker, manifest.json, offline destek, install prompt)
- Klavye kisayollari (Ctrl+K, Ctrl+F, Escape, Alt+Up/Down)
- Docker deploy (Dockerfile, docker-compose, Nginx, SSL)
- Monitoring (Sentry, Plausible)
- Responsive tasarim, keyboard navigation, loading skeleton'lar
- Input sanitization, CSRF korumasi, rate limiting
- SEO ve Open Graph tagleri

## Veritabani Semasi

Toplam ~20 tablo:

| Tablo | Aciklama |
|-------|----------|
| `profiles` | Kullanici profilleri |
| `servers` | Sunucular (guild) |
| `categories` | Kanal kategorileri |
| `channels` | Metin/ses/forum kanallari |
| `members` | Sunucu uyelikleri |
| `roles` | Sunucu rolleri |
| `member_roles` | Uye-rol iliskisi |
| `messages` | Mesajlar |
| `reactions` | Mesaj reaksiyonlari |
| `mentions` | Mention'lar |
| `read_states` | Okunmamis mesaj takibi |
| `dm_channels` | DM kanallari |
| `dm_participants` | DM katilimcilari |
| `friendships` | Arkadaslik sistemi |
| `voice_states` | Ses kanali durumlari |
| `notifications` | Bildirimler |
| `audit_logs` | Denetim kaydi |
| `bans` | Yasaklar |
| `channel_permission_overrides` | Kanal izin override'lari |
| `threads` | Thread'ler |
| `server_discovery` | Sunucu kesfet verileri |

## Permission Sistemi

Discord'un bitwise permission sistemi kullanilir:

```typescript
export const Permissions = {
  ADMINISTRATOR:        1n << 0n,
  MANAGE_SERVER:        1n << 1n,
  MANAGE_CHANNELS:      1n << 2n,
  MANAGE_ROLES:         1n << 3n,
  MANAGE_MESSAGES:      1n << 4n,
  KICK_MEMBERS:         1n << 5n,
  BAN_MEMBERS:          1n << 6n,
  SEND_MESSAGES:        1n << 7n,
  SEND_ATTACHMENTS:     1n << 8n,
  ADD_REACTIONS:        1n << 9n,
  MENTION_EVERYONE:     1n << 10n,
  CONNECT_VOICE:        1n << 11n,
  SPEAK:                1n << 12n,
  STREAM:               1n << 13n,
  MUTE_MEMBERS:         1n << 14n,
  DEAFEN_MEMBERS:       1n << 15n,
  MOVE_MEMBERS:         1n << 16n,
  USE_VOICE_ACTIVITY:   1n << 17n,
  CREATE_INVITE:        1n << 18n,
  CHANGE_NICKNAME:      1n << 19n,
  MANAGE_NICKNAMES:     1n << 20n,
  MANAGE_EMOJIS:        1n << 21n,
  VIEW_CHANNEL:         1n << 22n,
  VIEW_AUDIT_LOG:       1n << 23n,
  CREATE_THREADS:       1n << 24n,
  MANAGE_THREADS:       1n << 25n,
} as const;

export function hasPermission(userPerms: bigint, required: bigint): boolean {
  if (userPerms & Permissions.ADMINISTRATOR) return true;
  return (userPerms & required) === required;
}
```

## Onerilen Gelistirme Sirasi

1. **Temel uygulama:** Phase 0 → 1 → 2 → 3
2. **DM ve ses:** Phase 4 → 5
3. **Bildirim, moderasyon, ayarlar:** Phase 6 → 7 → 8
4. **Arama, dosya, thread:** Phase 9 → 10 → 11
5. **Optimizasyon ve deploy:** Phase 12

Her faz tamamlandiktan sonra test edilmeli, hata varsa duzeltilmeli ve bir sonraki faza ancak mevcut faz sorunsuz calistiginda gecilmelidir.

## Kurulum

```bash
# Bagimliliklari kur
npm install

# Gelistirme sunucusunu baslat
npm run dev

# Production build
npm run build

# Docker ile deploy
docker-compose up -d
```

## Ortam Degiskenleri

```env
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
LIVEKIT_API_KEY=
LIVEKIT_API_SECRET=
NEXT_PUBLIC_LIVEKIT_URL=
```

## Lisans

Bu proje ozel kullanim icindir.
