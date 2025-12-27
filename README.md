<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1"/>
  <title>BookLocal (Single-File Demo)</title>
  <style>
    :root{
      --bg:#0b0f14; --card:#121a27; --text:#e7eef9; --muted:#9bb0c9;
      --accent:#4aa3ff; --ok:#32d583; --warn:#f79009; --bad:#f04438;
      --border:rgba(255,255,255,0.10);
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial;
      color:var(--text);
      background: linear-gradient(180deg, #06090f 0%, var(--bg) 55%);
    }
    header{
      position:sticky; top:0;
      background:rgba(8,12,18,0.85);
      border-bottom:1px solid var(--border);
      backdrop-filter: blur(10px);
      z-index:10;
    }
    .wrap{max-width:1000px; margin:0 auto; padding:16px}
    .topbar{display:flex; justify-content:space-between; gap:14px; align-items:center}
    .brand{font-weight:800; letter-spacing:0.2px}
    .sub{color:var(--muted); font-size:13px; margin-top:4px}
    .pill{
      display:inline-block; margin-left:8px;
      padding:3px 8px; border-radius:999px;
      border:1px solid var(--border);
      color:var(--muted); font-size:12px; font-weight:600;
    }
    .top-actions{display:flex; gap:8px; flex-wrap:wrap}
    button{
      border-radius:12px;
      padding:10px 12px;
      border:1px solid rgba(74,163,255,0.35);
      background:rgba(74,163,255,0.12);
      color:var(--text);
      cursor:pointer;
    }
    button:hover{background:rgba(74,163,255,0.18)}
    button.ghost{
      border:1px solid var(--border);
      background:rgba(255,255,255,0.04);
    }
    button.ghost:hover{background:rgba(255,255,255,0.07)}
    button.danger{
      border-color: rgba(240,68,56,0.45);
      background: rgba(240,68,56,0.12);
    }
    button.danger:hover{background: rgba(240,68,56,0.18)}
    button:disabled{opacity:0.5; cursor:not-allowed}

    .card{
      border:1px solid var(--border);
      background:rgba(18,26,39,0.72);
      border-radius:16px;
      padding:14px;
    }
    label{display:block; font-size:12px; color:var(--muted)}
    input, select, textarea{
      width:100%;
      margin-top:6px;
      padding:10px 12px;
      border-radius:12px;
      border:1px solid var(--border);
      background:rgba(255,255,255,0.03);
      color:var(--text);
    }
    textarea{min-height:72px; resize:vertical}

    .grid3{display:grid; grid-template-columns:1fr 1fr 1fr; gap:10px}
    .grid2{display:grid; grid-template-columns:1fr 1fr; gap:12px}
    @media (max-width:900px){
      .grid3{grid-template-columns:1fr}
      .grid2{grid-template-columns:1fr}
      .topbar{align-items:flex-start; flex-direction:column}
    }

    .mt10{margin-top:10px}
    .mt12{margin-top:12px}
    .mt14{margin-top:14px}
    .actions{display:flex; align-items:flex-end}
    .note{color:var(--muted); font-size:13px}
    .badge{
      display:inline-block; padding:4px 9px;
      border-radius:999px; border:1px solid var(--border);
      color:var(--muted); font-size:12px; font-weight:650;
    }
    .flex{display:flex; align-items:center; gap:10px}
    .space{justify-content:space-between}
    hr{border:0; border-top:1px solid var(--border); margin:12px 0}

    .provider-title{font-size:16px; font-weight:800}
    .kpi{color:var(--muted); font-size:13px}
    .service{
      display:flex; justify-content:space-between; gap:10px;
      padding:10px 0; border-bottom:1px solid rgba(255,255,255,0.06);
    }
    .service:last-child{border-bottom:0}
    .money{font-weight:800}
    .small{font-size:12px; color:var(--muted)}
    .slot-grid{
      display:grid; grid-template-columns:repeat(4,1fr);
      gap:8px; margin-top:10px;
    }
    @media (max-width:900px){.slot-grid{grid-template-columns:repeat(2,1fr)}}
    .slot{
      padding:9px 10px;
      border-radius:12px;
      border:1px solid var(--border);
      background:rgba(255,255,255,0.03);
      cursor:pointer;
      text-align:center;
      user-select:none;
    }
    .slot:hover{border-color: rgba(74,163,255,0.55)}
    .slot.selected{
      border-color: rgba(50,213,131,0.7);
      background: rgba(50,213,131,0.10);
    }

    .good{color:var(--ok)}
    .warn{color:var(--warn)}
    .bad{color:var(--bad)}

    .modal{
      position:fixed; inset:0;
      background:rgba(0,0,0,0.55);
      display:flex; align-items:center; justify-content:center;
      padding:18px;
      z-index:20;
    }
    .modal-card{
      width:min(920px, 100%);
      max-height: 86vh;
      overflow:auto;
      border:1px solid var(--border);
      background:rgba(18,26,39,0.95);
      border-radius:16px;
      padding:14px;
    }
    .table{
      width:100%;
      border-collapse:collapse;
      margin-top:10px;
    }
    .table th, .table td{
      border-bottom:1px solid rgba(255,255,255,0.08);
      text-align:left;
      padding:10px 8px;
      vertical-align:top;
      font-size:13px;
    }
    .table th{color:var(--muted); font-weight:700}
  </style>
</head>
<body>
<header>
  <div class="wrap topbar">
    <div>
      <div class="brand">BookLocal <span class="pill">Single-file HTML Demo</span></div>
      <div class="sub">Search providers · Pick a time · “Pay” booking fee (simulated) · Get “SMS + Email” confirmations</div>
    </div>
    <div class="top-actions">
      <button class="ghost" id="openProviderInboxBtn">Provider Inbox</button>
      <button class="ghost" id="openNotificationsBtn">Customer Notifications</button>
      <button class="danger" id="resetDemoBtn" title="Clears demo bookings & notifications">Reset</button>
    </div>
  </div>
</header>

<main class="wrap">
  <section class="card">
    <div class="grid3">
      <label>
        Category
        <select id="category">
          <option value="">All</option>
          <option value="massage">Massage</option>
          <option value="barber">Barber</option>
        </select>
      </label>

      <label>
        Suburb
        <input id="suburb" placeholder="e.g. Bondi"/>
      </label>

      <label>
        Postcode
        <input id="postcode" placeholder="e.g. 2026" inputmode="numeric"/>
      </label>
    </div>

    <div class="grid3 mt12">
      <label>
        Date
        <input id="date" type="date"/>
      </label>

      <label>
        Sort
        <select id="sort">
          <option value="soonest">Soonest availability</option>
          <option value="price_low">Lowest price</option>
          <option value="rating_high">Highest rating</option>
        </select>
      </label>

      <div class="actions">
        <button id="searchBtn">Search providers</button>
      </div>
    </div>

    <div class="note mt10" id="statusMsg"></div>
  </section>

  <section class="grid2 mt14" id="results"></section>

  <section class="card mt14" id="bookingPanel" style="display:none;"></section>
</main>

<div class="modal" id="modalOverlay" style="display:none;">
  <div class="modal-card" id="modalCard"></div>
</div>

<script>
  // BookLocal HTML-only Demo (Single file)
  const $ = (id) => document.getElementById(id);

  const LS = {
    bookings: "booklocal_demo_bookings",
    customerNotifs: "booklocal_demo_customer_notifications",
    providerNotifs: "booklocal_demo_provider_notifications",
  };

  const BOOKING_FEE = 3.50;

  const DEMO_PROVIDERS = [
    {
      id: 1,
      business_name: "Salt & Stone Remedial",
      category: "massage",
      suburb: "Bondi",
      postcode: "2026",
      rating: 4.8,
      reviews: 214,
      bio: "Remedial + relaxation. Quiet rooms, no upsell circus.",
      services: [
        { id: 101, name: "Relaxation Massage", duration_min: 60, price: 105 },
        { id: 102, name: "Remedial Deep Tissue", duration_min: 60, price: 125 },
        { id: 103, name: "Sports Massage", duration_min: 45, price: 99 },
      ],
      availability: [
        { day: 1, start: "09:00", end: "17:00" },
        { day: 2, start: "10:00", end: "18:00" },
        { day: 4, start: "12:00", end: "19:00" },
        { day: 6, start: "09:00", end: "14:00" },
      ],
    },
    {
      id: 2,
      business_name: "Clip Theory Barber Co.",
      category: "barber",
      suburb: "Surry Hills",
      postcode: "2010",
      rating: 4.7,
      reviews: 391,
      bio: "Fast, clean fades. Book, arrive, leave looking expensive.",
      services: [
        { id: 201, name: "Standard Cut", duration_min: 30, price: 55 },
        { id: 202, name: "Skin Fade", duration_min: 45, price: 70 },
        { id: 203, name: "Cut + Beard", duration_min: 60, price: 95 },
      ],
      availability: [
        { day: 1, start: "10:00", end: "19:00" },
        { day: 3, start: "10:00", end: "19:00" },
        { day: 5, start: "10:00", end: "20:00" },
        { day: 6, start: "09:00", end: "15:00" },
      ],
    },
    {
      id: 3,
      business_name: "Ubud Calm (Mobile Massage)",
      category: "massage",
      suburb: "Newtown",
      postcode: "2042",
      rating: 4.9,
      reviews: 88,
      bio: "Mobile therapist. Brings table. Leaves silence behind.",
      services: [
        { id: 301, name: "Home Visit Relax", duration_min: 60, price: 140 },
        { id: 302, name: "Home Visit Deep Tissue", duration_min: 60, price: 160 },
      ],
      availability: [
        { day: 0, start: "10:00", end: "16:00" },
        { day: 2, start: "11:00", end: "18:00" },
        { day: 4, start: "11:00", end: "18:00" },
      ],
    },
    {
      id: 4,
      business_name: "Kingsway Cuts",
      category: "barber",
      suburb: "Parramatta",
      postcode: "2150",
      rating: 4.6,
      reviews: 517,
      bio: "Walk out sharper than your opinions.",
      services: [
        { id: 401, name: "Buzz Cut", duration_min: 20, price: 35 },
        { id: 402, name: "Cut", duration_min: 30, price: 50 },
        { id: 403, name: "Fade + Beard", duration_min: 60, price: 90 },
      ],
      availability: [
        { day: 2, start: "09:00", end: "17:00" },
        { day: 3, start: "09:00", end: "17:00" },
        { day: 4, start: "09:00", end: "17:00" },
        { day: 6, start: "09:00", end: "13:00" },
      ],
    },
  ];

  function todayISODate() {
    const d = new Date();
    const yyyy = d.getFullYear();
    const mm = String(d.getMonth() + 1).padStart(2, "0");
    const dd = String(d.getDate()).padStart(2, "0");
    return \`\${yyyy}-\${mm}-\${dd}\`;
  }

  function load(key, fallback) {
    try {
      const raw = localStorage.getItem(key);
      return raw ? JSON.parse(raw) : fallback;
    } catch {
      return fallback;
    }
  }
  function save(key, value) {
    localStorage.setItem(key, JSON.stringify(value));
  }

  function money(n) { return \`$\${Number(n).toFixed(2)}\`; }

  function minutesFromHHMM(hhmm) {
    const [h, m] = hhmm.split(":").map(Number);
    return h * 60 + m;
  }
  function hhmmFromMinutes(min) {
    const h = Math.floor(min / 60);
    const m = min % 60;
    return \`\${String(h).padStart(2, "0")}:\${String(m).padStart(2, "0")}\`;
  }
  function makeDateTime(dateStr, hhmm) {
    const [y, mo, d] = dateStr.split("-").map(Number);
    const [h, mi] = hhmm.split(":").map(Number);
    return new Date(y, mo - 1, d, h, mi, 0, 0);
  }
  function formatWhen(dt) {
    return dt.toLocaleString([], { weekday: "short", month: "short", day: "2-digit", hour: "2-digit", minute: "2-digit" });
  }

  function generateSlots(provider, service, dateStr) {
    const day = new Date(dateStr + "T00:00:00").getDay();
    const rules = provider.availability.filter(r => r.day === day);
    if (!rules.length) return [];

    const step = 15;
    const duration = service.duration_min;

    const bookings = load(LS.bookings, []).filter(b =>
      b.provider_id === provider.id && b.date === dateStr && b.status === "CONFIRMED"
    );

    const bookedRanges = bookings.map(b => ({
      start: minutesFromHHMM(b.time),
      end: minutesFromHHMM(b.time) + b.duration_min
    }));

    const slots = [];
    for (const r of rules) {
      let startMin = minutesFromHHMM(r.start);
      const endMin = minutesFromHHMM(r.end);

      startMin += (provider.id * 5) % 15; // tiny variation

      for (let t = startMin; t + duration <= endMin; t += step) {
        const now = new Date();
        const slotDateTime = makeDateTime(dateStr, hhmmFromMinutes(t));
        if (slotDateTime.getTime() < now.getTime() + 5 * 60000) continue;

        const overlaps = bookedRanges.some(br => t < br.end && (t + duration) > br.start);
        if (!overlaps) slots.push(hhmmFromMinutes(t));
      }
    }
    return slots.slice(0, 16);
  }

  function soonestSlotForProvider(provider, dateStr) {
    const svc = provider.services[0];
    const slots = generateSlots(provider, svc, dateStr);
    return slots[0] || null;
  }

  function scoreProvider(provider, sort, dateStr) {
    if (sort === "rating_high") return provider.rating * 1000 + provider.reviews;
    if (sort === "price_low") return -(Math.min(...provider.services.map(s => s.price)));
    const soonest = soonestSlotForProvider(provider, dateStr);
    return soonest ? -minutesFromHHMM(soonest) : -9999999;
  }

  function setStatus(text, cls = "") {
    const el = $("statusMsg");
    el.className = "note mt10 " + cls;
    el.textContent = text;
  }

  function renderProviders(list, dateStr, sort) {
    const results = $("results");
    if (!list.length) {
      results.innerHTML = "";
      setStatus("No providers found. Try suburb OR postcode, or change category.", "warn");
      return;
    }

    const sorted = [...list].sort((a, b) => scoreProvider(b, sort, dateStr) - scoreProvider(a, sort, dateStr));
    setStatus(\`Found \${sorted.length} providers. Pick one to book.\`, "good");

    results.innerHTML = sorted.map(p => {
      const soonest = soonestSlotForProvider(p, dateStr);
      const soonestLabel = soonest ? \`Next: \${soonest}\` : "No slots on this date";
      const fromPrice = Math.min(...p.services.map(s => s.price));
      return `
        <div class="card">
          <div class="flex space">
            <div>
              <div class="provider-title">${escapeHtml(p.business_name)}</div>
              <div class="kpi">${escapeHtml(p.suburb)} ${escapeHtml(p.postcode)} · <span class="badge">${escapeHtml(p.category)}</span></div>
            </div>
            <div style="text-align:right">
              <div class="badge">⭐ ${p.rating.toFixed(1)} (${p.reviews})</div>
              <div class="small mt10">From <span class="money">${money(fromPrice)}</span></div>
            </div>
          </div>
          <hr/>
          <div class="note">${escapeHtml(p.bio)}</div>
          <div class="flex space mt10">
            <div class="badge">${escapeHtml(soonestLabel)}</div>
            <button onclick="openBooking(${p.id})">View & Book</button>
          </div>
        </div>
      `;
    }).join("");
  }

  function openBooking(providerId) {
    const dateStr = $("date").value || todayISODate();
    const provider = DEMO_PROVIDERS.find(p => p.id === providerId);

    const panel = $("bookingPanel");
    panel.style.display = "block";

    const servicesHtml = provider.services.map(s => `
      <div class="service">
        <div>
          <div><strong>${escapeHtml(s.name)}</strong></div>
          <div class="small">${s.duration_min} min</div>
        </div>
        <div style="text-align:right">
          <div class="money">${money(s.price)}</div>
          <button class="ghost" onclick="selectService(${provider.id}, ${s.id})">Choose</button>
        </div>
      </div>
    `).join("");

    panel.innerHTML = `
      <div class="flex space">
        <div>
          <div class="provider-title">Book: ${escapeHtml(provider.business_name)}</div>
          <div class="kpi">${escapeHtml(provider.suburb)} ${escapeHtml(provider.postcode)} · ⭐ ${provider.rating.toFixed(1)} (${provider.reviews})</div>
        </div>
        <button class="ghost" onclick="scrollToTop()">Back to results</button>
      </div>
      <hr/>
      <div class="note">Step 1 — Choose a service</div>
      ${servicesHtml}
      <div id="bookingStep2" class="mt14"></div>
    `;

    selectService(provider.id, provider.services[0].id);
    panel.scrollIntoView({ behavior: "smooth", block: "start" });
  }

  function selectService(providerId, serviceId) {
    const dateStr = $("date").value || todayISODate();
    const provider = DEMO_PROVIDERS.find(p => p.id === providerId);
    const service = provider.services.find(s => s.id === serviceId);

    const slots = generateSlots(provider, service, dateStr);
    const slotHtml = slots.length
      ? `<div class="slot-grid">${slots.map(t => `<div class="slot" onclick="selectSlot('${t}')">${t}</div>`).join("")}</div>`
      : `<div class="note warn mt10">No available times for this service on this date. Try another date.</div>`;

    $("bookingStep2").innerHTML = `
      <div class="card" style="background:rgba(255,255,255,0.03); border-radius:14px;">
        <div class="note"><strong>Step 2 — Pick a time</strong></div>
        <div class="note mt10">Selected: <span class="badge">${escapeHtml(service.name)}</span> · ${service.duration_min} min · ${money(service.price)}</div>
        ${slotHtml}
        <div id="selectedTimeMsg" class="note mt10">${slots.length ? "Select a time slot to continue." : ""}</div>
        <div id="bookingStep3" class="mt14"></div>
      </div>
    `;

    window.__selected = { providerId, serviceId, dateStr, time: null };
  }

  function selectSlot(time) {
    document.querySelectorAll(".slot").forEach(s => s.classList.remove("selected"));
    const clicked = [...document.querySelectorAll(".slot")].find(el => el.textContent.trim() === time);
    if (clicked) clicked.classList.add("selected");

    window.__selected.time = time;
    $("selectedTimeMsg").innerHTML = `Selected time: <span class="badge">${escapeHtml(time)}</span>`;

    $("bookingStep3").innerHTML = `
      <div class="note"><strong>Step 3 — Your details</strong></div>
      <div class="grid3 mt12">
        <label>Full name <input id="custName" placeholder="Your name"/></label>
        <label>Email <input id="custEmail" placeholder="you@email.com" inputmode="email"/></label>
        <label>Mobile <input id="custPhone" placeholder="04xx xxx xxx" inputmode="tel"/></label>
      </div>

      <hr/>
      <div class="flex space">
        <div>
          <div class="note">Booking fee to confirm: <span class="badge">${money(BOOKING_FEE)}</span></div>
          <div class="small">Demo only: fee + confirmations are simulated (no real charge).</div>
        </div>
        <button onclick="confirmBooking()">Confirm booking</button>
      </div>

      <div id="bookMsg" class="note mt10"></div>
    `;
  }

  function confirmBooking() {
    const sel = window.__selected;
    if (!sel?.time) return;

    const name = $("custName").value.trim();
    const email = $("custEmail").value.trim();
    const phone = $("custPhone").value.trim();
    if (!name || !email || !phone) {
      $("bookMsg").textContent = "Please enter name, email, and mobile.";
      $("bookMsg").className = "note mt10 bad";
      return;
    }

    const provider = DEMO_PROVIDERS.find(p => p.id === sel.providerId);
    const service = provider.services.find(s => s.id === sel.serviceId);

    const booking = {
      id: String(Date.now()) + "_" + Math.floor(Math.random() * 9999),
      status: "CONFIRMED",
      provider_id: provider.id,
      provider_name: provider.business_name,
      category: provider.category,
      service_id: service.id,
      service_name: service.name,
      duration_min: service.duration_min,
      service_price: service.price,
      booking_fee: BOOKING_FEE,
      date: sel.dateStr,
      time: sel.time,
      customer_name: name,
      customer_email: email,
      customer_phone: phone,
      created_at: new Date().toISOString(),
    };

    const bookings = load(LS.bookings, []);
    bookings.push(booking);
    save(LS.bookings, bookings);

    const when = formatWhen(makeDateTime(booking.date, booking.time));
    const customerEmail = {
      at: new Date().toISOString(),
      channel: "Email",
      to: booking.customer_email,
      subject: "Booking Confirmed",
      message: `Confirmed: ${booking.provider_name}\nService: ${booking.service_name}\nWhen: ${when}\nBooking fee: ${money(booking.booking_fee)}`,
    };
    const customerSms = {
      at: new Date().toISOString(),
      channel: "SMS",
      to: booking.customer_phone,
      subject: "Booking Confirmed",
      message: `Booking confirmed: ${booking.provider_name} · ${booking.service_name} · ${when}`,
    };
    const providerNotif = {
      at: new Date().toISOString(),
      channel: "Provider Alert",
      to: booking.provider_name,
      subject: "New Booking",
      message: `${booking.customer_name} booked ${booking.service_name} · ${when}\nCustomer: ${booking.customer_phone}`,
    };

    const customerNotifs = load(LS.customerNotifs, []);
    customerNotifs.unshift(customerEmail, customerSms);
    save(LS.customerNotifs, customerNotifs);

    const providerNotifs = load(LS.providerNotifs, []);
    providerNotifs.unshift(providerNotif);
    save(LS.providerNotifs, providerNotifs);

    openModal(`
      <div class="flex space">
        <div>
          <div class="provider-title">Booking Confirmed ✅</div>
          <div class="note">Demo confirmation created (SMS + Email simulated).</div>
        </div>
        <button class="ghost" onclick="closeModal()">Close</button>
      </div>
      <hr/>
      <div class="grid2">
        <div class="card" style="background:rgba(255,255,255,0.03)">
          <div class="note"><strong>Booking</strong></div>
          <div class="mt10">
            <div><span class="badge">${escapeHtml(booking.provider_name)}</span></div>
            <div class="mt10"><strong>${escapeHtml(booking.service_name)}</strong> · ${booking.duration_min} min</div>
            <div class="mt10">When: <span class="badge">${escapeHtml(when)}</span></div>
            <div class="mt10">Service price: <strong>${money(booking.service_price)}</strong></div>
            <div class="mt10">Booking fee: <strong>${money(booking.booking_fee)}</strong> (demo)</div>
          </div>
        </div>

        <div class="card" style="background:rgba(255,255,255,0.03)">
          <div class="note"><strong>What happens next</strong></div>
          <ul class="note mt10" style="margin:0; padding-left:18px;">
            <li>Customer gets confirmation (Email + SMS)</li>
            <li>Provider gets booking notification</li>
          </ul>
          <div class="mt14 flex" style="gap:8px;">
            <button class="ghost" onclick="openCustomerNotifications()">View customer notifications</button>
            <button class="ghost" onclick="openProviderInbox()">View provider inbox</button>
          </div>
        </div>
      </div>
    `);

    runSearch();
    scrollToTop();
  }

  function openModal(html) {
    $("modalCard").innerHTML = html;
    $("modalOverlay").style.display = "flex";
  }
  function closeModal() {
    $("modalOverlay").style.display = "none";
    $("modalCard").innerHTML = "";
  }

  function openCustomerNotifications() {
    const items = load(LS.customerNotifs, []);
    openModal(`
      <div class="flex space">
        <div>
          <div class="provider-title">Customer Notifications</div>
          <div class="note">Simulated SMS + Email (stored in your browser).</div>
        </div>
        <button class="ghost" onclick="closeModal()">Close</button>
      </div>
      <table class="table">
        <thead>
          <tr><th>When</th><th>Channel</th><th>To</th><th>Message</th></tr>
        </thead>
        <tbody>
          ${items.length ? items.map(n => `
            <tr>
              <td>${escapeHtml(new Date(n.at).toLocaleString())}</td>
              <td><span class="badge">${escapeHtml(n.channel)}</span></td>
              <td>${escapeHtml(n.to)}</td>
              <td><strong>${escapeHtml(n.subject)}</strong><br/><span class="note">${escapeHtml(n.message)}</span></td>
            </tr>
          `).join("") : `<tr><td colspan="4" class="note">No notifications yet.</td></tr>`}
        </tbody>
      </table>
    `);
  }

  function openProviderInbox() {
    const items = load(LS.providerNotifs, []);
    openModal(`
      <div class="flex space">
        <div>
          <div class="provider-title">Provider Inbox</div>
          <div class="note">Simulated provider booking alerts.</div>
        </div>
        <button class="ghost" onclick="closeModal()">Close</button>
      </div>
      <table class="table">
        <thead>
          <tr><th>When</th><th>Provider</th><th>Alert</th></tr>
        </thead>
        <tbody>
          ${items.length ? items.map(n => `
            <tr>
              <td>${escapeHtml(new Date(n.at).toLocaleString())}</td>
              <td><span class="badge">${escapeHtml(n.to)}</span></td>
              <td><strong>${escapeHtml(n.subject)}</strong><br/><span class="note">${escapeHtml(n.message)}</span></td>
            </tr>
          `).join("") : `<tr><td colspan="3" class="note">No provider alerts yet.</td></tr>`}
        </tbody>
      </table>
    `);
  }

  function scrollToTop() { window.scrollTo({ top: 0, behavior: "smooth" }); }

  function escapeHtml(str) {
    return String(str).replace(/[&<>"']/g, (m) => ({
      "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#039;"
    }[m]));
  }

  function runSearch() {
    const category = $("category").value.trim();
    const suburb = $("suburb").value.trim();
    const postcode = $("postcode").value.trim();
    const dateStr = $("date").value || todayISODate();
    const sort = $("sort").value;

    let list = DEMO_PROVIDERS;
    if (category) list = list.filter(p => p.category === category);
    if (suburb) list = list.filter(p => p.suburb.toLowerCase() === suburb.toLowerCase());
    if (postcode) list = list.filter(p => p.postcode === postcode);

    renderProviders(list, dateStr, sort);
  }

  function resetDemo() {
    localStorage.removeItem(LS.bookings);
    localStorage.removeItem(LS.customerNotifs);
    localStorage.removeItem(LS.providerNotifs);
    $("bookingPanel").style.display = "none";
    setStatus("Demo reset. Bookings and notifications cleared.", "good");
    runSearch();
  }

  function init() {
    $("date").value = todayISODate();
    $("searchBtn").addEventListener("click", runSearch);
    $("openProviderInboxBtn").addEventListener("click", openProviderInbox);
    $("openNotificationsBtn").addEventListener("click", openCustomerNotifications);
    $("resetDemoBtn").addEventListener("click", resetDemo);

    $("modalOverlay").addEventListener("click", (e) => {
      if (e.target.id === "modalOverlay") closeModal();
    });

    setStatus("Ready. Search providers and make a demo booking to see confirmations.", "");
    runSearch();
  }

  init();
</script>
</body>
</html>
