/**
 * ============================================================
 *  BLOG PLATFORM WITH COMMENTS — COMPLETE SINGLE FILE
 * ============================================================
 *
 *  Combines:
 *   • Frontend  — React (Feed, Post, Write/Edit, Login, Register)
 *   • Backend   — Simulated Express REST API (in-memory, same logic)
 *   • Database  — In-memory SQLite-equivalent (users, posts, comments)
 *   • Auth      — JWT-style token (base64 encoded, verified in-memory)
 *
 *  REST API routes simulated:
 *   POST   /api/auth/register
 *   POST   /api/auth/login
 *   GET    /api/auth/me
 *   GET    /api/posts            ?search= &tag=
 *   GET    /api/posts/:id
 *   POST   /api/posts
 *   PUT    /api/posts/:id
 *   DELETE /api/posts/:id
 *   POST   /api/posts/:postId/comments
 *   DELETE /api/posts/:postId/comments/:id
 * ============================================================
 */

import { useState, useEffect, useCallback, createContext, useContext } from "react";

/* ═══════════════════════════════════════════════════════════
   LAYER 1 — DATABASE  (in-memory, mirrors SQLite schema)
   ═══════════════════════════════════════════════════════════ */
let _users = [
  { id: 1, name: "Alex Rivera",  email: "alex@demo.com",  password: btoa("demo123"), avatar: "AR", created_at: "2026-06-01T08:00:00Z" },
  { id: 2, name: "Sam Lee",      email: "sam@demo.com",   password: btoa("demo123"), avatar: "SL", created_at: "2026-06-02T09:00:00Z" },
];
let _posts = [
  {
    id: 1, author_id: 1, title: "Getting Started with React Hooks",
    body: "React Hooks revolutionised the way we write components. useState and useEffect alone replace most class-component patterns, keeping logic co-located and readable. In this post we explore the most useful hooks and common pitfalls to avoid when migrating from class-based components.",
    tags: JSON.stringify(["React", "JavaScript"]),
    created_at: "2026-06-20T09:00:00Z", updated_at: "2026-06-20T09:00:00Z",
  },
  {
    id: 2, author_id: 2, title: "Why RESTful APIs Still Matter in 2026",
    body: "Despite the rise of GraphQL and tRPC, REST remains the backbone of most public APIs. Its simplicity, cacheability, and broad tooling support make it the default choice for teams that need interoperability. This post covers best practices for versioning, error shapes, and pagination.",
    tags: JSON.stringify(["API", "Backend"]),
    created_at: "2026-06-22T14:00:00Z", updated_at: "2026-06-22T14:00:00Z",
  },
];
let _comments = [
  { id: 1, post_id: 1, author_id: 2, body: "Great intro! The useEffect cleanup section really clicked for me.", created_at: "2026-06-20T10:30:00Z" },
];
let _nextUserId    = 3;
let _nextPostId    = 3;
let _nextCommentId = 2;

/* ═══════════════════════════════════════════════════════════
   LAYER 2 — AUTH  (bcrypt → btoa, JWT → base64 JSON token)
   ═══════════════════════════════════════════════════════════ */
const JWT_SECRET = "blog_dev_secret";

function hashPassword(plain)        { return btoa(plain); }
function checkPassword(plain, hash) { return btoa(plain) === hash; }

function signToken(user) {
  const payload = { id: user.id, name: user.name, email: user.email, avatar: user.avatar, exp: Date.now() + 7 * 86400000 };
  return btoa(JSON.stringify(payload));
}
function verifyToken(token) {
  try {
    const payload = JSON.parse(atob(token));
    if (payload.exp < Date.now()) throw new Error("expired");
    return payload;
  } catch { throw new Error("Invalid token"); }
}

/* ═══════════════════════════════════════════════════════════
   LAYER 3 — REST API HANDLERS  (mirrors Express route logic)
   ═══════════════════════════════════════════════════════════ */
function apiError(msg, status = 400) { const e = new Error(msg); e.status = status; throw e; }

function getCurrentUser(token) {
  if (!token) apiError("Authentication required.", 401);
  return verifyToken(token);
}

function parseTags(raw) { try { return JSON.parse(raw); } catch { return []; } }

function formatPost(post, withComments = false) {
  const author = _users.find(u => u.id === post.author_id);
  const commentCount = _comments.filter(c => c.post_id === post.id).length;
  const out = {
    id: post.id, title: post.title, body: post.body,
    tags: parseTags(post.tags),
    createdAt: post.created_at, updatedAt: post.updated_at,
    author: { id: author.id, name: author.name, avatar: author.avatar },
    commentCount,
  };
  if (withComments) {
    out.comments = _comments
      .filter(c => c.post_id === post.id)
      .sort((a, b) => a.created_at.localeCompare(b.created_at))
      .map(c => {
        const ca = _users.find(u => u.id === c.author_id);
        return { id: c.id, body: c.body, createdAt: c.created_at, author: { id: ca.id, name: ca.name, avatar: ca.avatar } };
      });
  }
  return out;
}

const API = {
  /* ── Auth ─────────────────────────────────────────── */
  "POST /api/auth/register": ({ body }) => {
    const { name, email, password } = body;
    if (!name?.trim())         apiError("name is required.");
    if (!email?.includes("@")) apiError("Invalid email address.");
    if (!password || password.length < 6) apiError("Password must be at least 6 characters.");
    if (_users.find(u => u.email === email.toLowerCase())) apiError("Email already registered.", 409);
    const avatar = name.trim().split(" ").map(w => w[0]).join("").toUpperCase().slice(0, 2);
    const user = { id: _nextUserId++, name: name.trim(), email: email.toLowerCase(), password: hashPassword(password), avatar, created_at: new Date().toISOString() };
    _users.push(user);
    const token = signToken(user);
    const { password: _, ...safe } = user;
    return { token, user: safe };
  },

  "POST /api/auth/login": ({ body }) => {
    const { email, password } = body;
    if (!email || !password) apiError("email and password are required.");
    const user = _users.find(u => u.email === email.toLowerCase());
    if (!user || !checkPassword(password, user.password)) apiError("Incorrect email or password.", 401);
    const token = signToken(user);
    const { password: _, ...safe } = user;
    return { token, user: safe };
  },

  "GET /api/auth/me": ({ token }) => {
    const cur = getCurrentUser(token);
    const user = _users.find(u => u.id === cur.id);
    if (!user) apiError("User not found.", 404);
    const { password: _, ...safe } = user;
    return { user: safe };
  },

  /* ── Posts ────────────────────────────────────────── */
  "GET /api/posts": ({ query }) => {
    const { search = "", tag = "" } = query || {};
    let posts = _posts.filter(p => {
      const matchSearch = !search || p.title.toLowerCase().includes(search.toLowerCase()) || p.body.toLowerCase().includes(search.toLowerCase());
      const matchTag    = !tag    || parseTags(p.tags).some(t => t.toLowerCase() === tag.toLowerCase());
      return matchSearch && matchTag;
    });
    posts = [...posts].sort((a, b) => b.created_at.localeCompare(a.created_at));
    return { posts: posts.map(p => formatPost(p)) };
  },

  "GET /api/posts/:id": ({ params }) => {
    const post = _posts.find(p => p.id === +params.id);
    if (!post) apiError("Post not found.", 404);
    return { post: formatPost(post, true) };
  },

  "POST /api/posts": ({ body, token }) => {
    const cur = getCurrentUser(token);
    const { title, body: bodyText, tags = [] } = body;
    if (!title?.trim())    apiError("title is required.");
    if (!bodyText?.trim()) apiError("body is required.");
    if (!Array.isArray(tags)) apiError("tags must be an array.");
    const now = new Date().toISOString();
    const post = { id: _nextPostId++, author_id: cur.id, title: title.trim(), body: bodyText.trim(), tags: JSON.stringify(tags), created_at: now, updated_at: now };
    _posts.push(post);
    return { post: formatPost(post, true) };
  },

  "PUT /api/posts/:id": ({ params, body, token }) => {
    const cur  = getCurrentUser(token);
    const post = _posts.find(p => p.id === +params.id);
    if (!post)              apiError("Post not found.", 404);
    if (post.author_id !== cur.id) apiError("You can only edit your own posts.", 403);
    if (body.title) post.title = body.title.trim();
    if (body.body)  post.body  = body.body.trim();
    if (Array.isArray(body.tags)) post.tags = JSON.stringify(body.tags);
    post.updated_at = new Date().toISOString();
    return { post: formatPost(post, true) };
  },

  "DELETE /api/posts/:id": ({ params, token }) => {
    const cur  = getCurrentUser(token);
    const idx  = _posts.findIndex(p => p.id === +params.id);
    if (idx === -1)               apiError("Post not found.", 404);
    if (_posts[idx].author_id !== cur.id) apiError("You can only delete your own posts.", 403);
    _posts.splice(idx, 1);
    _comments = _comments.filter(c => c.post_id !== +params.id);
    return { message: "Post deleted successfully." };
  },

  /* ── Comments ─────────────────────────────────────── */
  "POST /api/posts/:postId/comments": ({ params, body, token }) => {
    const cur  = getCurrentUser(token);
    const post = _posts.find(p => p.id === +params.postId);
    if (!post)               apiError("Post not found.", 404);
    if (!body.body?.trim())  apiError("Comment body is required.");
    const comment = { id: _nextCommentId++, post_id: post.id, author_id: cur.id, body: body.body.trim(), created_at: new Date().toISOString() };
    _comments.push(comment);
    const author = _users.find(u => u.id === cur.id);
    return { comment: { id: comment.id, body: comment.body, createdAt: comment.created_at, author: { id: author.id, name: author.name, avatar: author.avatar } } };
  },

  "DELETE /api/posts/:postId/comments/:id": ({ params, token }) => {
    const cur = getCurrentUser(token);
    const idx = _comments.findIndex(c => c.id === +params.id && c.post_id === +params.postId);
    if (idx === -1)                         apiError("Comment not found.", 404);
    if (_comments[idx].author_id !== cur.id) apiError("You can only delete your own comments.", 403);
    _comments.splice(idx, 1);
    return { message: "Comment deleted." };
  },
};

/* Route dispatcher — matches :param patterns */
function dispatch(method, path, opts = {}) {
  for (const key of Object.keys(API)) {
    const [km, kp] = key.split(" ");
    if (km !== method) continue;
    const paramNames = [];
    const re = new RegExp("^" + kp.replace(/:(\w+)/g, (_, n) => { paramNames.push(n); return "([^/]+)"; }) + "$");
    const m  = path.match(re);
    if (!m) continue;
    const params = Object.fromEntries(paramNames.map((n, i) => [n, m[i + 1]]));
    return API[key]({ ...opts, params });
  }
  throw Object.assign(new Error("Route not found."), { status: 404 });
}

/* ═══════════════════════════════════════════════════════════
   LAYER 4 — API CLIENT  (mirrors frontend/src/api/client.js)
   ═══════════════════════════════════════════════════════════ */
function getToken() { return sessionStorage.getItem("token"); }

function request(method, path, body = null, query = null) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {                          // simulate network latency
      try {
        const result = dispatch(method, path, { body, query, token: getToken() });
        resolve(result);
      } catch (e) { reject(e); }
    }, 80);
  });
}

const api = {
  auth: {
    register: (name, email, password)  => request("POST", "/api/auth/register", { name, email, password }),
    login:    (email, password)        => request("POST", "/api/auth/login",    { email, password }),
    me:       ()                       => request("GET",  "/api/auth/me"),
  },
  posts: {
    list:   (search = "", tag = "")        => request("GET",    "/api/posts",       null, { search, tag }),
    get:    (id)                           => request("GET",    `/api/posts/${id}`),
    create: (title, body, tags)            => request("POST",   "/api/posts",       { title, body, tags }),
    update: (id, title, body, tags)        => request("PUT",    `/api/posts/${id}`, { title, body, tags }),
    delete: (id)                           => request("DELETE", `/api/posts/${id}`),
  },
  comments: {
    add:    (postId, body)      => request("POST",   `/api/posts/${postId}/comments`,       { body }),
    delete: (postId, commentId) => request("DELETE", `/api/posts/${postId}/comments/${commentId}`),
  },
};

/* ═══════════════════════════════════════════════════════════
   LAYER 5 — AUTH CONTEXT  (mirrors frontend/src/context/AuthContext.js)
   ═══════════════════════════════════════════════════════════ */
const AuthCtx = createContext(null);

function AuthProvider({ children }) {
  const [user, setUser]       = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const token = sessionStorage.getItem("token");
    if (!token) { setLoading(false); return; }
    api.auth.me()
      .then(({ user }) => setUser(user))
      .catch(() => sessionStorage.removeItem("token"))
      .finally(() => setLoading(false));
  }, []);

  async function register(name, email, password) {
    const { token, user } = await api.auth.register(name, email, password);
    sessionStorage.setItem("token", token);
    setUser(user);
  }
  async function login(email, password) {
    const { token, user } = await api.auth.login(email, password);
    sessionStorage.setItem("token", token);
    setUser(user);
  }
  function logout() { sessionStorage.removeItem("token"); setUser(null); }

  return <AuthCtx.Provider value={{ user, login, logout, register, loading }}>{children}</AuthCtx.Provider>;
}

function useAuth() { return useContext(AuthCtx); }

/* ═══════════════════════════════════════════════════════════
   LAYER 6 — DESIGN TOKENS
   ═══════════════════════════════════════════════════════════ */
const C = {
  bg: "#0F1117", surface: "#1A1D27", card: "#222536",
  border: "#2E3148", accent: "#6C63FF", accentH: "#8A84FF",
  green: "#22C55E", red: "#EF4444",
  text: "#E8E9F3", muted: "#8B8FA8", tag: "#2A2D42",
};

const S = {
  btn:   { padding: "8px 18px", borderRadius: 8, border: "none", cursor: "pointer", fontWeight: 600, fontSize: 13, transition: "opacity .15s" },
  ghost: { background: "transparent", color: C.muted, border: `1px solid ${C.border}` },
  input: { width: "100%", background: C.surface, border: `1px solid ${C.border}`, borderRadius: 8, padding: "10px 14px", color: C.text, fontSize: 14, outline: "none", boxSizing: "border-box" },
  card:  { background: C.card, border: `1px solid ${C.border}`, borderRadius: 12, padding: "22px 24px", transition: "border-color .18s" },
  label: { fontSize: 13, fontWeight: 600, color: C.muted, display: "flex", flexDirection: "column" },
  link:  { background: "none", border: "none", color: C.accent, cursor: "pointer", fontWeight: 600, fontSize: 13, padding: 0 },
};

/* ═══════════════════════════════════════════════════════════
   LAYER 7 — UI COMPONENTS
   ═══════════════════════════════════════════════════════════ */
const AVATAR_COLORS = ["#6C63FF","#22C55E","#F59E0B","#EF4444","#06B6D4"];
function Avatar({ label = "?", size = 34 }) {
  const color = AVATAR_COLORS[(label.charCodeAt(0) || 0) % AVATAR_COLORS.length];
  return (
    <div style={{ width: size, height: size, minWidth: size, borderRadius: "50%", background: color, color: "#fff", display: "flex", alignItems: "center", justifyContent: "center", fontSize: size * 0.36, fontWeight: 700 }}>
      {label}
    </div>
  );
}

function Tag({ label }) {
  return <span style={{ background: C.tag, color: C.muted, padding: "3px 10px", borderRadius: 20, fontSize: 11, fontWeight: 600 }}>{label}</span>;
}

function Toast({ msg, type }) {
  return (
    <div style={{ position: "fixed", bottom: 24, left: "50%", transform: "translateX(-50%)", background: type === "ok" ? C.green : C.red, color: "#fff", padding: "10px 22px", borderRadius: 30, fontSize: 13, fontWeight: 600, zIndex: 999, boxShadow: "0 4px 20px #0008", whiteSpace: "nowrap" }}>
      {msg}
    </div>
  );
}

function Spinner() {
  return <p style={{ color: C.muted, textAlign: "center", marginTop: 60 }}>Loading…</p>;
}

function fmtDate(iso) {
  return new Date(iso).toLocaleDateString("en-US", { month: "short", day: "numeric", year: "numeric" });
}

/* ═══════════════════════════════════════════════════════════
   LAYER 8 — PAGES
   ═══════════════════════════════════════════════════════════ */

/* ── NavBar ──────────────────────────────────────────────── */
function NavBar({ onFeed, onWrite, onLogin, onRegister }) {
  const { user, logout } = useAuth();
  return (
    <nav style={{ background: C.surface, borderBottom: `1px solid ${C.border}`, position: "sticky", top: 0, zIndex: 50 }}>
      <div style={{ maxWidth: 760, margin: "0 auto", padding: "0 16px", display: "flex", alignItems: "center", height: 56, gap: 12 }}>
        <button onClick={onFeed} style={{ background: "none", border: "none", cursor: "pointer", display: "flex", alignItems: "center", gap: 8, color: C.text }}>
          <span style={{ color: C.accent, fontSize: 22, fontWeight: 800 }}>✦</span>
          <span style={{ fontSize: 17, fontWeight: 700 }}>Inkwell</span>
        </button>
        <div style={{ flex: 1 }} />
        {user ? (
          <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
            <button onClick={onWrite} style={{ ...S.btn, background: C.accent, color: "#fff" }}>+ Write</button>
            <Avatar label={user.avatar} size={32} />
            <span style={{ fontSize: 13, color: C.muted }}>{user.name.split(" ")[0]}</span>
            <button onClick={() => { logout(); onFeed(); }} style={{ ...S.btn, ...S.ghost }}>Out</button>
          </div>
        ) : (
          <div style={{ display: "flex", gap: 8 }}>
            <button onClick={onLogin}    style={{ ...S.btn, ...S.ghost }}>Log in</button>
            <button onClick={onRegister} style={{ ...S.btn, background: C.accent, color: "#fff" }}>Sign up</button>
          </div>
        )}
      </div>
    </nav>
  );
}

/* ── Feed ────────────────────────────────────────────────── */
function FeedPage({ onOpen, onWrite, onToast }) {
  const { user } = useAuth();
  const [posts, setPosts]   = useState([]);
  const [search, setSearch] = useState("");
  const [busy, setBusy]     = useState(true);

  const load = useCallback(() => {
    setBusy(true);
    api.posts.list(search)
      .then(d => setPosts(d.posts))
      .catch(e => onToast(e.message, "err"))
      .finally(() => setBusy(false));
  }, [search]); // eslint-disable-line

  useEffect(() => { const t = setTimeout(load, 250); return () => clearTimeout(t); }, [load]);

  return (
    <div>
      <div style={{ display: "flex", gap: 10, marginBottom: 24 }}>
        <input value={search} onChange={e => setSearch(e.target.value)} placeholder="Search posts or tags…" style={{ ...S.input, flex: 1 }} />
        {user && <button onClick={onWrite} style={{ ...S.btn, background: C.accent, color: "#fff" }}>+ New Post</button>}
      </div>
      {busy && <Spinner />}
      {!busy && posts.length === 0 && (
        <div style={{ textAlign: "center", color: C.muted, marginTop: 60 }}>
          <div style={{ fontSize: 42, marginBottom: 10 }}>📭</div>
          <p>{user ? "No posts yet. Be the first to write one!" : "No posts found. Log in to start writing."}</p>
        </div>
      )}
      <div style={{ display: "flex", flexDirection: "column", gap: 16 }}>
        {posts.map(p => (
          <div key={p.id} onClick={() => onOpen(p.id)} style={{ ...S.card, cursor: "pointer" }}
            onMouseEnter={e => e.currentTarget.style.borderColor = C.accent}
            onMouseLeave={e => e.currentTarget.style.borderColor = C.border}>
            <div style={{ display: "flex", gap: 10, alignItems: "center", marginBottom: 12 }}>
              <Avatar label={p.author.avatar} />
              <div>
                <div style={{ fontWeight: 600, fontSize: 13 }}>{p.author.name}</div>
                <div style={{ color: C.muted, fontSize: 11 }}>{fmtDate(p.createdAt)}</div>
              </div>
            </div>
            <h2 style={{ margin: "0 0 8px", fontSize: 17, fontWeight: 700, lineHeight: 1.35 }}>{p.title}</h2>
            <p style={{ margin: "0 0 14px", color: C.muted, fontSize: 14, lineHeight: 1.6, display: "-webkit-box", WebkitLineClamp: 2, WebkitBoxOrient: "vertical", overflow: "hidden" }}>{p.body}</p>
            <div style={{ display: "flex", gap: 6, flexWrap: "wrap", alignItems: "center" }}>
              {p.tags.map(t => <Tag key={t} label={t} />)}
              <span style={{ marginLeft: "auto", color: C.muted, fontSize: 12 }}>💬 {p.commentCount}</span>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}

/* ── Post Detail ─────────────────────────────────────────── */
function PostPage({ id, onBack, onEdit, onDeleted, onToast }) {
  const { user } = useAuth();
  const [post, setPost]           = useState(null);
  const [commentText, setComment] = useState("");
  const [busy, setBusy]           = useState(true);
  const [submitting, setSubmit]   = useState(false);

  useEffect(() => {
    setBusy(true);
    api.posts.get(id)
      .then(d => setPost(d.post))
      .catch(e => onToast(e.message, "err"))
      .finally(() => setBusy(false));
  }, [id]); // eslint-disable-line

  async function handleDelete() {
    if (!window.confirm("Delete this post?")) return;
    try { await api.posts.delete(id); onDeleted(); }
    catch (e) { onToast(e.message, "err"); }
  }

  async function submitComment() {
    if (!commentText.trim()) return;
    setSubmit(true);
    try {
      const { comment } = await api.comments.add(id, commentText.trim());
      setPost(p => ({ ...p, comments: [...p.comments, comment], commentCount: p.commentCount + 1 }));
      setComment("");
    } catch (e) { onToast(e.message, "err"); }
    finally { setSubmit(false); }
  }

  async function deleteComment(commentId) {
    try {
      await api.comments.delete(id, commentId);
      setPost(p => ({ ...p, comments: p.comments.filter(c => c.id !== commentId), commentCount: p.commentCount - 1 }));
    } catch (e) { onToast(e.message, "err"); }
  }

  if (busy) return <Spinner />;
  if (!post) return <p style={{ color: C.muted }}>Post not found.</p>;
  const isAuthor = user && user.id === post.author.id;

  return (
    <div>
      <button onClick={onBack} style={{ ...S.btn, ...S.ghost, marginBottom: 20 }}>← Back</button>

      <div style={{ ...S.card, marginBottom: 24 }}>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 16 }}>
          <div style={{ display: "flex", gap: 10, alignItems: "center" }}>
            <Avatar label={post.author.avatar} size={38} />
            <div>
              <div style={{ fontWeight: 600 }}>{post.author.name}</div>
              <div style={{ color: C.muted, fontSize: 12 }}>{fmtDate(post.createdAt)}</div>
            </div>
          </div>
          {isAuthor && (
            <div style={{ display: "flex", gap: 8 }}>
              <button onClick={() => onEdit(post)} style={{ ...S.btn, ...S.ghost }}>Edit</button>
              <button onClick={handleDelete} style={{ ...S.btn, background: C.red + "22", color: C.red, border: `1px solid ${C.red}44` }}>Delete</button>
            </div>
          )}
        </div>
        <h1 style={{ margin: "0 0 12px", fontSize: 22, fontWeight: 800, lineHeight: 1.3 }}>{post.title}</h1>
        <div style={{ display: "flex", gap: 6, flexWrap: "wrap", marginBottom: 18 }}>
          {post.tags.map(t => <Tag key={t} label={t} />)}
        </div>
        <p style={{ margin: 0, fontSize: 15, lineHeight: 1.8, color: "#CBD0E8" }}>{post.body}</p>
      </div>

      <div style={S.card}>
        <h3 style={{ margin: "0 0 18px", fontSize: 15, fontWeight: 700 }}>💬 Comments ({post.comments.length})</h3>

        {post.comments.length === 0 && <p style={{ color: C.muted, fontSize: 14, marginBottom: 20 }}>No comments yet — be the first!</p>}

        <div style={{ display: "flex", flexDirection: "column", gap: 12, marginBottom: 20 }}>
          {post.comments.map(c => (
            <div key={c.id} style={{ background: C.surface, borderRadius: 10, padding: "14px 16px", display: "flex", gap: 12 }}>
              <Avatar label={c.author.avatar} size={30} />
              <div style={{ flex: 1 }}>
                <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 6 }}>
                  <span style={{ fontWeight: 600, fontSize: 13 }}>
                    {c.author.name}
                    <span style={{ color: C.muted, fontWeight: 400, fontSize: 11, marginLeft: 8 }}>{fmtDate(c.createdAt)}</span>
                  </span>
                  {user && user.id === c.author.id && (
                    <button onClick={() => deleteComment(c.id)} style={{ background: "none", border: "none", color: C.red, cursor: "pointer", fontSize: 12 }}>✕</button>
                  )}
                </div>
                <p style={{ margin: 0, fontSize: 14, color: "#CBD0E8", lineHeight: 1.6 }}>{c.body}</p>
              </div>
            </div>
          ))}
        </div>

        {user ? (
          <div>
            <textarea value={commentText} onChange={e => setComment(e.target.value)} placeholder="Add a comment…" rows={3} style={{ ...S.input, resize: "vertical", marginBottom: 10 }} />
            <button onClick={submitComment} disabled={!commentText.trim() || submitting} style={{ ...S.btn, background: C.accent, color: "#fff" }}>
              {submitting ? "Posting…" : "Post Comment"}
            </button>
          </div>
        ) : (
          <p style={{ color: C.muted, fontSize: 13 }}>Log in to leave a comment.</p>
        )}
      </div>
    </div>
  );
}

/* ── Write / Edit ────────────────────────────────────────── */
function WritePage({ existing, onDone, onCancel }) {
  const { user } = useAuth();
  const [title, setTitle] = useState(existing?.title || "");
  const [body, setBody]   = useState(existing?.body  || "");
  const [tags, setTags]   = useState((existing?.tags || []).join(", "));
  const [err, setErr]     = useState("");
  const [busy, setBusy]   = useState(false);

  if (!user) return <p style={{ color: C.muted }}>Please log in to write posts.</p>;

  async function handleSubmit() {
    setErr("");
    if (!title.trim()) { setErr("Title is required."); return; }
    if (!body.trim())  { setErr("Body is required."); return; }
    const tagList = tags.split(",").map(t => t.trim()).filter(Boolean);
    setBusy(true);
    try {
      if (existing) {
        const { post } = await api.posts.update(existing.id, title.trim(), body.trim(), tagList);
        onDone(post.id);
      } else {
        const { post } = await api.posts.create(title.trim(), body.trim(), tagList);
        onDone(post.id);
      }
    } catch (e) { setErr(e.message); }
    finally { setBusy(false); }
  }

  return (
    <div style={S.card}>
      <h2 style={{ margin: "0 0 24px", fontSize: 18, fontWeight: 800 }}>{existing ? "Edit Post" : "New Post"}</h2>
      <div style={{ display: "flex", flexDirection: "column", gap: 14 }}>
        <label style={S.label}>Title *
          <input value={title} onChange={e => setTitle(e.target.value)} placeholder="Your post title" style={{ ...S.input, marginTop: 6 }} />
        </label>
        <label style={S.label}>Body *
          <textarea value={body} onChange={e => setBody(e.target.value)} placeholder="Write your post content…" rows={9} style={{ ...S.input, marginTop: 6, resize: "vertical" }} />
        </label>
        <label style={S.label}>Tags (comma-separated)
          <input value={tags} onChange={e => setTags(e.target.value)} placeholder="React, JavaScript, Tips" style={{ ...S.input, marginTop: 6 }} />
        </label>
        {err && <p style={{ color: C.red, fontSize: 13, margin: 0 }}>{err}</p>}
        <div style={{ display: "flex", gap: 10, marginTop: 4 }}>
          <button onClick={handleSubmit} disabled={busy} style={{ ...S.btn, background: C.accent, color: "#fff" }}>
            {busy ? "Saving…" : (existing ? "Save Changes" : "Publish")}
          </button>
          <button onClick={onCancel} style={{ ...S.btn, ...S.ghost }}>Cancel</button>
        </div>
      </div>
    </div>
  );
}

/* ── Login ───────────────────────────────────────────────── */
function LoginPage({ onDone, onSwitch }) {
  const { login } = useAuth();
  const [email, setEmail]   = useState("");
  const [password, setPass] = useState("");
  const [err, setErr]       = useState("");
  const [busy, setBusy]     = useState(false);

  async function handle() {
    setErr(""); setBusy(true);
    try { await login(email, password); onDone(); }
    catch (e) { setErr(e.message); }
    finally { setBusy(false); }
  }

  return (
    <div style={{ maxWidth: 400, margin: "40px auto" }}>
      <div style={{ ...S.card, display: "flex", flexDirection: "column", gap: 14 }}>
        <h2 style={{ margin: "0 0 8px", fontSize: 20, fontWeight: 800 }}>Welcome back</h2>
        <label style={S.label}>Email
          <input type="email" value={email} onChange={e => setEmail(e.target.value)} placeholder="you@example.com" style={{ ...S.input, marginTop: 5 }} />
        </label>
        <label style={S.label}>Password
          <input type="password" value={password} onChange={e => setPass(e.target.value)} placeholder="••••••••" style={{ ...S.input, marginTop: 5 }} />
        </label>
        {err && <p style={{ color: C.red, fontSize: 13, margin: 0 }}>{err}</p>}
        <button onClick={handle} disabled={busy} style={{ ...S.btn, background: C.accent, color: "#fff", width: "100%" }}>
          {busy ? "Logging in…" : "Log in"}
        </button>
        <p style={{ textAlign: "center", fontSize: 13, color: C.muted }}>
          No account? <button onClick={onSwitch} style={S.link}>Sign up</button>
        </p>
        <p style={{ textAlign: "center", fontSize: 11, color: C.muted }}>Demo: alex@demo.com / demo123</p>
      </div>
    </div>
  );
}

/* ── Register ────────────────────────────────────────────── */
function RegisterPage({ onDone, onSwitch }) {
  const { register } = useAuth();
  const [name, setName]     = useState("");
  const [email, setEmail]   = useState("");
  const [password, setPass] = useState("");
  const [err, setErr]       = useState("");
  const [busy, setBusy]     = useState(false);

  async function handle() {
    setErr(""); setBusy(true);
    try { await register(name, email, password); onDone(); }
    catch (e) { setErr(e.message); }
    finally { setBusy(false); }
  }

  return (
    <div style={{ maxWidth: 400, margin: "40px auto" }}>
      <div style={{ ...S.card, display: "flex", flexDirection: "column", gap: 14 }}>
        <h2 style={{ margin: "0 0 8px", fontSize: 20, fontWeight: 800 }}>Create account</h2>
        <label style={S.label}>Full Name
          <input value={name} onChange={e => setName(e.target.value)} placeholder="Jane Doe" style={{ ...S.input, marginTop: 5 }} />
        </label>
        <label style={S.label}>Email
          <input type="email" value={email} onChange={e => setEmail(e.target.value)} placeholder="you@example.com" style={{ ...S.input, marginTop: 5 }} />
        </label>
        <label style={S.label}>Password
          <input type="password" value={password} onChange={e => setPass(e.target.value)} placeholder="Min 6 characters" style={{ ...S.input, marginTop: 5 }} />
        </label>
        {err && <p style={{ color: C.red, fontSize: 13, margin: 0 }}>{err}</p>}
        <button onClick={handle} disabled={busy} style={{ ...S.btn, background: C.accent, color: "#fff", width: "100%" }}>
          {busy ? "Creating…" : "Create account"}
        </button>
        <p style={{ textAlign: "center", fontSize: 13, color: C.muted }}>
          Have an account? <button onClick={onSwitch} style={S.link}>Log in</button>
        </p>
      </div>
    </div>
  );
}

/* ═══════════════════════════════════════════════════════════
   LAYER 9 — ROOT APP
   ═══════════════════════════════════════════════════════════ */
function BlogApp() {
  const { user, logout, loading } = useAuth();
  const [screen, setScreen]           = useState("feed");
  const [activePostId, setActivePostId] = useState(null);
  const [editPost, setEditPost]         = useState(null);
  const [toast, setToast]               = useState(null);

  function showToast(msg, type = "ok") {
    setToast({ msg, type });
    setTimeout(() => setToast(null), 2800);
  }
  function goFeed()  { setScreen("feed"); setEditPost(null); }
  function openPost(id) { setActivePostId(id); setScreen("post"); }
  function goWrite(post = null) { setEditPost(post); setScreen("write"); }

  if (loading) return <div style={{ minHeight: "100vh", background: C.bg }}><Spinner /></div>;

  return (
    <div style={{ minHeight: "100vh", background: C.bg, color: C.text, fontFamily: "'Inter',system-ui,sans-serif" }}>
      <NavBar
        onFeed={goFeed}
        onWrite={() => goWrite()}
        onLogin={() => setScreen("login")}
        onRegister={() => setScreen("register")}
      />
      {toast && <Toast {...toast} />}
      <main style={{ maxWidth: 760, margin: "0 auto", padding: "26px 16px 80px" }}>
        {screen === "feed"     && <FeedPage     onOpen={openPost} onWrite={goWrite} onToast={showToast} />}
        {screen === "post"     && <PostPage     id={activePostId} onBack={goFeed} onEdit={p => goWrite(p)} onDeleted={() => { goFeed(); showToast("Post deleted."); }} onToast={showToast} />}
        {screen === "write"    && <WritePage    existing={editPost} onDone={id => { openPost(id); showToast(editPost ? "Post updated!" : "Post published!"); }} onCancel={() => editPost ? openPost(editPost.id) : goFeed()} />}
        {screen === "login"    && <LoginPage    onDone={() => { goFeed(); showToast("Welcome back!"); }} onSwitch={() => setScreen("register")} />}
        {screen === "register" && <RegisterPage onDone={() => { goFeed(); showToast("Account created!"); }} onSwitch={() => setScreen("login")} />}
      </main>
    </div>
  );
}

export default function App() {
  return <AuthProvider><BlogApp /></AuthProvider>;
}
