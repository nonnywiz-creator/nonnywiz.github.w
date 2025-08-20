>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Kingsley's Contact Page</title>
  <style>
    :root{
      --bg:#0f1724;
      --card:#0b1220;
      --muted:#94a3b8;
      --accent:#60a5fa;
      --glass: rgba(255,255,255,0.03);
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, Arial;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      background:linear-gradient(180deg, #071023 0%, #07112a 100%);
      color:#e6eef8;
      font-size:16px;
      line-height:1.45;
      min-height:100vh;
      display:grid;
      place-items:center;
      padding:24px;
    }
    .container{
      width:100%;
      max-width:920px;
      background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:14px;
      padding:28px;
      box-shadow: 0 8px 30px rgba(2,6,23,0.6);
      border:1px solid rgba(255,255,255,0.03);
      display:grid;
      gap:20px;
    }
    header{
      display:flex;
      justify-content:space-between;
      align-items:center;
      gap:16px;
    }
    header h1{ margin:0; font-size:1.15rem }
    header p{ margin:0; color:var(--muted); font-size:0.95rem }
    .grid{ display:grid; grid-template-columns: 1fr 320px; gap:18px; }
    .card{
      background:var(--card);
      padding:16px;
      border-radius:12px;
      border:1px solid rgba(255,255,255,0.02);
      box-shadow: 0 4px 16px rgba(2,6,23,0.5);
    }
    form label{ display:block; font-weight:600; margin-bottom:8px; color:#dbeafe; }
    input, textarea{
      width:100%;
      padding:10px 12px;
      border-radius:8px;
      border:1px solid rgba(255,255,255,0.04);
      background:var(--glass);
      color: #e6eef8;
      outline:none;
    }
    textarea{ resize:vertical; min-height:60px; }
    .muted{ color:var(--muted); font-size:0.95rem; margin-top:8px }
    button{
      background:linear-gradient(90deg,var(--accent), #3b82f6);
      color:#05243a;
      padding:10px 14px;
      border:none;
      border-radius:10px;
      font-weight:700;
      cursor:pointer;
      box-shadow: 0 6px 18px rgba(59,130,246,0.12);
    }
    button:active{ transform:translateY(1px) }
    @media (max-width:900px){ .grid{ grid-template-columns: 1fr; } }
    .toast{
      position:fixed;
      left:50%;
      transform:translateX(-50%);
      bottom:36px;
      background:#0b1220;
      padding:10px 16px;
      border-radius:10px;
      border:1px solid rgba(255,255,255,0.04);
      color:var(--muted);
      display:none;
      z-index:1200;
      box-shadow: 0 6px 24px rgba(2,6,23,0.6);
    }
    footer{
      text-align:center;
      margin-top:20px;
      font-size:0.9rem;
      color:var(--muted);
    }
  </style>
</head>
<body>
  <main class="container">
    <header>
      <div>
        <h1>Kingsley's Web Snippet</h1>
        <p class="muted">Portfolio contact page with form + my details</p>
      </div>
      <small class="muted">HTML • CSS • JS</small>
    </header>

    <section class="grid">
      <!-- Left side: form -->
      <div class="card">
        <h2>Contact Form</h2>
        <form id="contactForm" novalidate>
          <label for="name">Full name</label>
          <input id="name" name="name" type="text" placeholder="e.g. Chinonso Kingsley" required />

          <label for="email" style="margin-top:12px">Email</label>
          <input id="email" name="email" type="email" placeholder="you@example.com" required />

          <label for="address" style="margin-top:12px">Address</label>
          <input id="address" name="address" type="text" placeholder="Street, City, State" />

          <label for="message" style="margin-top:12px">Message</label>
          <textarea id="message" name="message" rows="4" placeholder="Write a short message..."></textarea>

          <div style="display:flex; justify-content:space-between; align-items:center; margin-top:14px;">
            <small id="formStatus" class="muted">Client-side only — no data sent.</small>
            <button type="submit">Send</button>
          </div>
        </form>
      </div>

      <!-- Right side: info + preview -->
      <aside class="card">
        <h3>My Contact</h3>
        <div class="muted">
          📞 <strong>09063643738</strong><br/>
          ✉️ <a href="mailto:nonnywiz@gmail.com" style="color:#60a5fa; text-decoration:none;">nonnywiz@gmail.com</a>
        </div>

        <hr style="border:none; height:1px; background:rgba(255,255,255,0.02); margin:12px 0" />

        <h4>Form Preview</h4>
        <div id="preview" class="muted">No messages yet.</div>
      </aside>
    </section>
  </main>

  <footer>
    <p>© 2025 Kingsley — 📞 09063643738 | ✉️ nonnywiz@gmail.com</p>
  </footer>

  <div id="toast" class="toast"></div>

  <script>
    // JavaScript for form + preview + toast
    (function(){
      const form = document.getElementById('contactForm');
      const preview = document.getElementById('preview');
      const toast = document.getElementById('toast');
      const status = document.getElementById('formStatus');

      function showToast(text, timeout=2500){
        toast.textContent = text;
        toast.style.display = 'block';
        setTimeout(()=> toast.style.display = 'none', timeout);
      }

      function isValidEmail(email){
        return /^[^\\s@]+@[^\\s@]+\\.[^\\s@]+$/.test(String(email).toLowerCase());
      }

      form.addEventListener('submit', function(e){
        e.preventDefault();
        const name = form.name.value.trim();
        const email = form.email.value.trim();
        const address = form.address.value.trim();
        const message = form.message.value.trim();

        if(!name){ showToast('Please enter your name'); return; }
        if(!email || !isValidEmail(email)){ showToast('Invalid email'); return; }

        preview.innerHTML = `
          <strong>${escapeHtml(name)}</strong><br/>
          <span style="color:var(--muted)">${escapeHtml(email)}</span><br/>
          ${address ? '<em>Address:</em> ' + escapeHtml(address) + '<br/>' : ''}
          ${message ? '<em>Message:</em> ' + escapeHtml(message) : ''}
        `;

        status.textContent = 'Form data previewed locally.';
        showToast('Preview updated!');
        form.message.value = '';
      });

      function escapeHtml(s){
        return s.replace(/[&<>"]/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]));
      }
    })();
  </script>
</body>
</html>