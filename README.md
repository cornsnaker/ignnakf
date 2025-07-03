
# 📥 Instagram Downloader API (SaveInsta Proxy)

A powerful PHP-based API endpoint that proxies [SaveInsta.to](https://saveinsta.to) to download **Instagram posts, reels, stories, and profiles** — returning clean structured JSON with thumbnails and resolutions.

> 🔐 No Instagram login, no cookies, no browser automation.

---

## ⚙️ How It Works

This script performs the following:

1. ✅ Accepts `?url=https://instagram.com/...` via GET param
2. ✅ Validates only *safe* Instagram links (profile, post, reel, story)
3. 🧠 Scrapes `saveinsta.to/en/highlights` for dynamic tokens (JavaScript-generated)
4. 🔓 Bypasses Cloudflare with short-lived access token via `userverify` endpoint
5. 📦 Sends final content request with all tokens to `ajaxSearch`
6. 🔍 Parses the returned HTML using `DOMDocument` + `DOMXPath`
7. 📤 Returns a structured response like this:

```json
{
  "success": true,
  "media": {
    "results_images": 1,
    "results_videos": 0,
    "images": [ ... ],
    "videos": [ ... ]
  }
}
````

---

## 🔗 Example Usage

```bash
GET /insta-downloader.php?url=https://www.instagram.com/p/XXXXXXXX/
```

**Response (for image post)**:

```json
{
  "thumb_url": "https://...",
  "resolutions_count": 2,
  "resolution": [
    { "Original": "..." },
    { "HD": "..." }
  ]
}
```

**Response (for video post)**:

```json
{
  "thumb_url": "https://...",
  "video_src": "https://dl.snapcdn.app/...",
  "resolutions_count": 0,
  "resolution": []
}
```

---

## 🚀 Getting Started

### 🔧 Requirements

* PHP 7.4 or newer
* cURL enabled
* A server or local environment (XAMPP, Laravel Valet, etc.)

### ▶️ How to Run

```bash
1. Upload insta-downloader.php to your web server
2. Access in browser or programmatically:
   http://yourdomain.com/insta-downloader.php?url=https://instagram.com/reel/xxxx
```

---

## 🔒 URL Validation

The script only accepts the following Instagram URL types:

* ✅ Profile: `https://instagram.com/username/`
* ✅ Post: `https://instagram.com/p/abc123/`
* ✅ Reel: `https://instagram.com/reel/xyz456/`
* ✅ Story: `https://instagram.com/stories/username/789...`

> ❌ If the URL is not valid, you'll get:
>
> ```json
> { "error": "Only profile, post, reel, or story URLs are supported." }
> ```

---

## 🛠️ Configuration & Customization

| Task                 | What to Edit                                             |
| -------------------- | -------------------------------------------------------- |
| Force fixed delay    | `$delay_mode = "fixed"`                                  |
| Delay settings       | `$fixed_delay`, `$delay_min`, `$delay_max`               |
| Allow more URL types | Add patterns to `$validPatterns[]`                       |
| Save media locally   | Use `file_put_contents()` in `rebuild_and_parse_media()` |
| Add logging          | Use `error_log()` or save to `.log` file                 |
| Customize output     | Modify final `json_encode()` section                     |

> 💡 You can easily extend this to support downloading or storing files locally.

---

## 📦 Output Format

Each media item contains:

### Images

```json
{
  "thumb_url": "https://...",
  "resolutions_count": 3,
  "resolution": [
    { "Original": "..." },
    { "HD": "..." },
    { "Preview": "..." }
  ]
}
```

### Videos

```json
{
  "thumb_url": "https://...",
  "video_src": "https://...",
  "resolutions_count": 0,
  "resolution": []
}
```

---

## 📌 TODOs and Cool Experiments

> ✅ TODO: Add endpoint rate limiting (e.g., 5 reqs/min IP-based)

> 🧪 TODO: Extend this to support IGTV or multi-image carousel parsing

> 🔐 TODO: Add token-based protection to prevent abuse

> 🧰 TODO: Build a lightweight frontend UI using fetch() + TailwindCSS

> 🎯 TODO: Cache token requests for 10–15s for performance

---

## ⚠️ Limitations

* ⚠ Rate-limited by SaveInsta if used heavily
* 🔄 Tokens (`k_token`, `k_exp`, `cftoken`) can change anytime
* 🚫 No login/auth API — this is strictly public-facing scraping
* 💥 Instagram may eventually change how link previews work

---

## 📜 License

MIT License — Free to use, modify, and share.

> Please use ethically and responsibly.

Built by **Syn Devs** – coders of clean APIs and rebels against algorithmic exploitation.

---

## ☕ Like It?

If this helped you, consider giving the repo a 🌟 or forking it to your toolkit.


