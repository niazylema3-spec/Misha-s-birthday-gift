<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>С днём рождения, Миша!</title>
  <style>
    :root {
      --bg1: #ffe9f5;
      --bg2: #fff7d6;
      --pink: #ff7eb6;
      --pink-deep: #ff4d9a;
      --purple: #8f7bff;
      --blue: #8ad7ff;
      --mint: #a6f0c4;
      --yellow: #ffd76a;
      --text: #542c4f;
      --glass: rgba(255, 255, 255, 0.45);
      --shadow: 0 18px 35px rgba(180, 90, 160, 0.18);
    }

    * { box-sizing: border-box; }

    html, body {
      margin: 0;
      min-height: 100%;
      font-family: "Segoe UI", Tahoma, sans-serif;
      background: linear-gradient(135deg, var(--bg1), var(--bg2));
      color: var(--text);
      overflow-x: hidden;
    }

    body::before, body::after {
      content: "";
      position: fixed;
      width: 200px;
      height: 200px;
      border-radius: 50%;
      filter: blur(25px);
      opacity: 0.6;
      z-index: 0;
    }

    body::before {
      background: rgba(255, 126, 182, 0.35);
      top: -50px;
      left: -30px;
      animation: float1 10s ease-in-out infinite alternate;
    }

    body::after {
      background: rgba(138, 215, 255, 0.35);
      right: -30px;
      bottom: -30px;
      animation: float2 12s ease-in-out infinite alternate;
    }

    @keyframes float1 {
      0% { transform: translate(0, 0) scale(1); }
      100% { transform: translate(30px, 30px) scale(1.08); }
    }

    @keyframes float2 {
      0% { transform: translate(0, 0) scale(1); }
      100% { transform: translate(-30px, -30px) scale(1.1); }
    }

    .overlay {
      position: fixed;
      inset: 0;
      background: radial-gradient(circle at top, rgba(255,255,255,0.9), rgba(255, 211, 234, 0.7));
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 100;
      transition: opacity 0.6s ease;
    }

    .overlay.hidden {
      opacity: 0;
      pointer-events: none;
    }

    .lock-box {
      background: rgba(255,255,255,0.7);
      border: 2px solid rgba(255, 125, 182, 0.45);
      box-shadow: var(--shadow);
      border-radius: 28px;
      width: min(92vw, 650px);
      padding: 28px 22px 20px;
      text-align: center;
      backdrop-filter: blur(8px);
    }

    .lock-box h1 {
      margin: 0;
      font-size: clamp(1.8rem, 4vw, 3rem);
      color: var(--pink-deep);
    }

    .lock-box p {
      margin: 10px 0 18px;
      font-size: 1.05rem;
      color: var(--text);
    }

    .memory-grid {
      display: grid;
      grid-template-columns: repeat(4, minmax(58px, 1fr));
      gap: 12px;
      margin: 16px auto 18px;
      max-width: 380px;
    }

    .memory-card {
      appearance: none;
      border: none;
      border-radius: 16px;
      height: 72px;
      font-size: 2rem;
      background: linear-gradient(135deg, #ffd9ea, #f8f0ff);
      box-shadow: inset 0 0 0 2px rgba(143, 123, 255, 0.15);
      cursor: pointer;
      transition: transform 0.18s ease, box-shadow 0.18s ease, background 0.2s ease;
    }

    .memory-card:hover {
      transform: translateY(-2px);
    }

    .memory-card.matched {
      background: linear-gradient(135deg, #d8f7dd, #cff5f0);
      box-shadow: inset 0 0 0 2px rgba(76, 182, 100, 0.22);
      cursor: default;
    }

    .memory-card.flipped {
      background: linear-gradient(135deg, #fffefc, #f1f7ff);
      transform: scale(0.98);
    }

    .pin-area {
      display: none;
      margin-top: 12px;
      align-items: center;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    .pin-area.visible {
      display: flex;
    }

    .pin-display {
      display: flex;
      gap: 10px;
      background: rgba(255,255,255,0.7);
      padding: 10px 14px;
      border-radius: 14px;
      border: 1px solid rgba(143, 123, 255, 0.2);
      box-shadow: inset 0 0 0 1px rgba(143, 123, 255, 0.08);
    }

    .pin-display .digit {
      width: 22px;
      height: 22px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--pink), var(--purple));
      display: inline-flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-weight: bold;
      font-size: 0.8rem;
    }

    .pin-input {
      width: 135px;
      padding: 12px 14px;
      border-radius: 12px;
      border: 2px solid rgba(143, 123, 255, 0.25);
      font-size: 1.1rem;
      text-align: center;
      outline: none;
      background: white;
    }

    .unlock-btn, .section-btn, .modal-btn {
      border: none;
      border-radius: 999px;
      padding: 12px 20px;
      font-size: 1rem;
      font-weight: 700;
      cursor: pointer;
      transition: transform 0.18s ease, box-shadow 0.18s ease;
      box-shadow: 0 12px 20px rgba(143, 123, 255, 0.18);
    }

    .unlock-btn, .modal-btn {
      background: linear-gradient(135deg, var(--pink), var(--purple));
      color: white;
    }

    .unlock-btn:hover, .section-btn:hover, .modal-btn:hover {
      transform: translateY(-2px);
    }

    .app {
      position: relative;
      z-index: 1;
      padding: 28px 18px 60px;
      max-width: 1100px;
      margin: 0 auto;
    }

    .hero {
      display: grid;
      grid-template-columns: 1.2fr 0.8fr;
      gap: 26px;
      align-items: center;
      padding: 20px 0 12px;
    }

    .hero-card {
      background: rgba(255,255,255,0.48);
      border: 1px solid rgba(255, 125, 182, 0.22);
      border-radius: 28px;
      box-shadow: var(--shadow);
      padding: 26px;
      backdrop-filter: blur(10px);
    }

    .badge {
      display: inline-block;
      padding: 7px 12px;
      border-radius: 999px;
      background: linear-gradient(135deg, #fff0c5, #ffd0ec);
      font-weight: 700;
      font-size: 0.8rem;
      letter-spacing: 0.06em;
    }

    .hero h2 {
      font-size: clamp(2.2rem, 5vw, 4rem);
      margin: 16px 0 12px;
      line-height: 1.08;
      color: var(--pink-deep);
    }

    .hero p {
      font-size: 1.04rem;
      line-height: 1.7;
      margin: 0 0 20px;
    }

    .hero-illustration {
      display: flex;
      align-items: center;
      justify-content: center;
      min-height: 300px;
      background: linear-gradient(135deg, rgba(255, 255, 255, 0.4), rgba(255, 245, 184, 0.5));
      border-radius: 30px;
      box-shadow: var(--shadow);
      border: 1px solid rgba(255, 125, 182, 0.18);
      position: relative;
      overflow: hidden;
    }

    .gift {
      position: relative;
      width: 150px;
      height: 150px;
      animation: wobble 3s ease-in-out infinite;
    }

    @keyframes wobble {
      0%, 100% { transform: rotate(-4deg) translateY(0); }
      50% { transform: rotate(4deg) translateY(-8px); }
    }

    .gift-box {
      position: absolute;
      inset: 18px;
      border-radius: 22px;
      background: linear-gradient(135deg, #ffd86a, #ffae6d);
      box-shadow: 0 18px 20px rgba(255, 170, 95, 0.32);
    }

    .gift-box::before, .gift-box::after {
      content: "";
      position: absolute;
      background: rgba(255,255,255,0.5);
    }

    .gift-box::before {
      left: 50%;
      top: 0;
      width: 18px;
      height: 100%;
      transform: translateX(-50%);
      border-radius: 20px;
    }

    .gift-box::after {
      top: 50%;
      left: 0;
      width: 100%;
      height: 18px;
      transform: translateY(-50%);
      border-radius: 20px;
    }

    .gift-ribbon {
      position: absolute;
      inset: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 4rem;
      color: #fff2b3;
      text-shadow: 0 8px 20px rgba(255, 180, 80, 0.2);
    }

    .steps {
      margin-top: 30px;
      display: grid;
      grid-template-columns: repeat(2, minmax(250px, 1fr));
      gap: 22px;
    }

    .step-card {
      position: relative;
      background: rgba(255,255,255,0.48);
      border: 1px solid rgba(255, 125, 182, 0.18);
      border-radius: 26px;
      box-shadow: var(--shadow);
      padding: 20px 18px 18px;
      overflow: hidden;
      min-height: 220px;
    }

    .step-card.locked::before {
      content: "🔒";
      position: absolute;
      top: 16px;
      right: 18px;
      font-size: 1.5rem;
      opacity: 0.8;
    }

    .step-card h3 {
      margin: 8px 0 8px;
      color: var(--purple);
      font-size: 1.35rem;
    }

    .step-card p {
      margin: 0 0 14px;
      line-height: 1.6;
      font-size: 0.98rem;
    }

    .step-card.open {
      background: linear-gradient(135deg, rgba(255,255,255,0.7), rgba(230,255,238,0.6));
      border-color: rgba(110, 210, 140, 0.35);
    }

    .step-card.open::before {
      content: "✅";
      position: absolute;
      top: 14px;
      right: 18px;
      font-size: 1.5rem;
    }

    .section-btn {
      background: linear-gradient(135deg, var(--pink), var(--purple));
      color: white;
    }

    .hidden-content {
      display: none;
      margin-top: 12px;
      padding-top: 12px;
      border-top: 1px dashed rgba(143, 123, 255, 0.25);
    }

    .step-card.open .hidden-content {
      display: block;
    }

    .gallery {
      display: grid;
      grid-template-columns: repeat(3, minmax(120px, 1fr));
      gap: 10px;
    }

    .photo {
      height: 120px;
      border-radius: 16px;
      background: linear-gradient(135deg, #f8d7eb, #d7f6ff);
      border: 1px solid rgba(143, 123, 255, 0.16);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 3rem;
      box-shadow: inset 0 0 0 2px rgba(255,255,255,0.4);
    }

    .playlist-box {
      display: grid;
      gap: 12px;
    }

    .song {
      display: flex;
      align-items: center;
      justify-content: space-between;
      background: rgba(255,255,255,0.56);
      border-radius: 12px;
      padding: 10px 12px;
      border: 1px solid rgba(143, 123, 255, 0.12);
    }

    .letter {
      background: rgba(255,255,255,0.7);
      border-radius: 18px;
      padding: 18px;
      line-height: 1.8;
      border: 1px solid rgba(143, 123, 255, 0.12);
      color: var(--text);
    }

    .final-box {
      background: linear-gradient(135deg, rgba(255, 236, 169, 0.8), rgba(255, 196, 216, 0.8));
      padding: 22px 18px;
      border-radius: 18px;
      border: 1px solid rgba(255, 125, 182, 0.2);
      text-align: center;
      position: relative;
      overflow: hidden;
    }

    .final-box::before, .final-box::after {
      content: "✨";
      position: absolute;
      font-size: 2rem;
      top: 12px;
      animation: confetti 2.8s ease-in-out infinite;
    }

    .final-box::before { left: 18px; }
    .final-box::after { right: 18px; animation-delay: 0.6s; }

    @keyframes confetti {
      0%, 100% { transform: translateY(0) scale(1); opacity: 0.8; }
      50% { transform: translateY(-12px) scale(1.2); opacity: 1; }
    }

    .surprise {
      font-size: clamp(1.8rem, 4vw, 3rem);
      margin: 8px 0;
      color: #8a3f88;
      font-weight: 900;
      text-shadow: 0 10px 25px rgba(255, 118, 168, 0.18);
    }

    .modal {
      position: fixed;
      inset: 0;
      background: rgba(68, 31, 58, 0.38);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 200;
      padding: 16px;
    }

    .modal.show {
      display: flex;
    }

    .modal-card {
      width: min(92vw, 430px);
      background: rgba(255,255,255,0.92);
      border-radius: 24px;
      box-shadow: var(--shadow);
      border: 1px solid rgba(143, 123, 255, 0.15);
      padding: 22px 18px 18px;
      text-align: center;
    }

    .modal-card h4 {
      margin: 0 0 10px;
      font-size: 1.3rem;
      color: var(--pink-deep);
    }

    .modal-card p {
      margin: 0 0 16px;
      line-height: 1.6;
      color: var(--text);
    }

    .modal-input {
      width: 100%;
      border: 2px solid rgba(143, 123, 255, 0.2);
      border-radius: 12px;
      padding: 12px 14px;
      font-size: 1.05rem;
      outline: none;
      margin-bottom: 12px;
      background: white;
    }

    .modal-btn {
      width: 100%;
      margin-top: 4px;
    }

    @media (max-width: 760px) {
      .hero {
        grid-template-columns: 1fr;
      }

      .steps {
        grid-template-columns: 1fr;
      }

      .gallery {
        grid-template-columns: repeat(2, minmax(120px, 1fr));
      }
    }
  </style>
</head>
<body>

  <div class="overlay" id="entryOverlay">
    <div class="lock-box">
      <h1>Секретный вход</h1>
      <p>Собери все пары, чтобы открыть секретный код и узнать, кто ждёт тебя внутри ✨</p>

      <div class="memory-grid" id="memoryGrid"></div>

      <div class="pin-area" id="pinArea">
        <div class="pin-display">
          <span class="digit">·</span>
          <span class="digit">·</span>
          <span class="digit">·</span>
          <span class="digit">·</span>
        </div>
        <input class="pin-input" id="pinInput" type="password" maxlength="4" placeholder="Код" />
        <button class="unlock-btn" id="unlockBtn">Открыть</button>
      </div>
    </div>
  </div>

  <main class="app" id="app" style="display: none;">
    <section class="hero">
      <div class="hero-card">
        <span class="badge">Невероятный подарок</span>
        <h2>С днём рождения, Миша!</h2>
        <p>
          Этот день создан специально для тебя: сказка, улыбки, волнение, тепло и самый яркий сюрприз.
          Всё это ждёт тебя — просто открой следующий шаг и окунись в маленькое чудо.
        </p>
        <button class="section-btn" onclick="speak('С днём рождения, Миша! Твоя история начинается прямо сейчас.')">
          Включить голосовое поздравление
        </button>
      </div>

      <div class="hero-illustration">
        <div class="gift">
          <div class="gift-box"></div>
          <div class="gift-ribbon">🎁</div>
        </div>
      </div>
    </section>

    <section class="steps">
      <article class="step-card locked" data-key="album">
        <h3>Шаг 1 · Фотоальбом</h3>
        <p>Удивительная подборка моментов, где каждый кадр хранит историю и тепло.</p>
        <button class="section-btn">Открыть фотоальбом</button>
        <div class="hidden-content">
          <div class="gallery">
            <div class="photo">📸</div>
            <div class="photo">😊</div>
            <div class="photo">🌟</div>
            <div class="photo">💫</div>
            <div class="photo">🎉</div>
            <div class="photo">💖</div>
          </div>
        </div>
      </article>

      <article class="step-card locked" data-key="playlist">
        <h3>Шаг 2 · Плейлист</h3>
        <p>Музыка, которая звучит как твоё настроение: весело, тепло и по-настоящему душевно.</p>
        <button class="section-btn">Открыть плейлист</button>
        <div class="hidden-content">
          <div class="playlist-box">
            <div class="song"><span>1. Вдохновляющая мелодия</span><span>🎵</span></div>
            <div class="song"><span>2. Для тёплых воспоминаний</span><span>🎶</span></div>
            <div class="song"><span>3. Супер энергия на весь день</span><span>🔥</span></div>
            <div class="song"><span>4. Песенка для счастья</span><span>✨</span></div>
          </div>
        </div>
      </article>

      <article class="step-card locked" data-key="letter">
        <h3>Шаг 3 · Письмо от друга</h3>
        <p>Слова, которые похожи на тёплое объятие — искренние, простые и очень важные.</p>
        <button class="section-btn">Открыть письмо</button>
        <div class="hidden-content">
          <div class="letter">
            Миша, ты — один из тех людей, которые делают пространство вокруг себя светлее.
            Ты умеешь создавать тепло там, где многие просто проходят мимо.
            В твоей жизни есть сердце, смех, дружба, искренность и невероятная душевность.
            Пусть каждый день приносит тебе радость, а рядом всегда будут те, кто ценит тебя так, как ты достоин.
            Ты заслуживаешь самого лучшего — любви, счастья, моментов, которые хочется помнить навсегда.
            <br><br>
            С днём рождения! Пусть всё хорошее, что тебе хочется, обязательно приходит к тебе.
          </div>
        </div>
      </article>

      <article class="step-card locked" data-key="surprise">
        <h3>Шаг 4 · Главный сюрприз</h3>
        <p>Последняя и самая важная часть праздника: то, ради чего всё и начиналось.</p>
        <button class="section-btn">Открыть сюрприз</button>
        <div class="hidden-content">
          <div class="final-box">
            <div class="surprise">Ты особенный</div>
            <p>Пусть в твоей жизни всегда будет место для улыбок, чудес, неожиданных подарков и людей, которые любят тебя по-настоящему.</p>
            <h3>💖 И пусть лучший подарок — это счастье, которое приходит к тебе каждый день 💖</h3>
          </div>
        </div>
      </article>
    </section>
  </main>

  <div class="modal" id="modal">
    <div class="modal-card">
      <h4 id="modalTitle">Загадка</h4>
      <p id="modalQuestion">Вопрос</p>
      <input id="modalInput" class="modal-input" type="text" placeholder="Напиши ответ" />
      <button class="modal-btn" id="modalBtn">Проверить</button>
    </div>
  </div>

  <script>
    const memorySymbols = ["🎂", "💖", "✨", "🎁"];
    const board = document.getElementById("memoryGrid");
    const pinArea = document.getElementById("pinArea");
    const pinInput = document.getElementById("pinInput");
    const unlockBtn = document.getElementById("unlockBtn");
    const entryOverlay = document.getElementById("entryOverlay");
    const app = document.getElementById("app");
    const modal = document.getElementById("modal");
    const modalTitle = document.getElementById("modalTitle");
    const modalQuestion = document.getElementById("modalQuestion");
    const modalInput = document.getElementById("modalInput");
    const modalBtn = document.getElementById("modalBtn");

    const correctPin = "4207";
    let openedCards = [];
    let matchedPairs = 0;
    let activeChallengeKey = null;

    const challenges = {
      album: { title: "Фотоальбом", question: "Как называется раздел, где живут самые тёплые и счастливые кадры?", answer: "альбом" },
      playlist: { title: "Плейлист", question: "Что помогает звучать мелодиям в одном настроении?", answer: "плейлист" },
      letter: { title: "Письмо", question: "Что обычно читают с душой и хранят в сердце?", answer: "письмо" },
      surprise: { title: "Главный сюрприз", question: "Что дарит радость, когда ты меньше всего ждёшь?", answer: "сюрприз" }
    };

    function initMemoryBoard() {
      const cards = [...memorySymbols, ...memorySymbols]
        .sort(() => Math.random() - 0.5)
        .map((symbol, index) => {
          const btn = document.createElement("button");
          btn.className = "memory-card";
          btn.textContent = "❔";
          btn.dataset.symbol = symbol;
          btn.dataset.index = index;
          btn.setAttribute("aria-label", "Скрытая карточка");

          btn.addEventListener("click", () => {
            if (btn.classList.contains("matched") || btn.classList.contains("flipped")) return;

            btn.textContent = symbol;
            btn.classList.add("flipped");
            openedCards.push(btn);

            if (openedCards.length === 2) {
              const [first, second] = openedCards;
              if (first.dataset.symbol === second.dataset.symbol) {
                first.classList.add("matched");
                second.classList.add("matched");
                matchedPairs++;
                openedCards = [];
                if (matchedPairs === memorySymbols.length) {
                  setTimeout(() => {
                    pinArea.classList.add("visible");
                    pinInput.focus();
                  }, 350);
                }
              } else {
                setTimeout(() => {
                  first.textContent = "❔";
                  second.textContent = "❔";
                  first.classList.remove("flipped");
                  second.classList.remove("flipped");
                  openedCards = [];
                }, 500);
              }
            }
          });

          return btn;
        });

      board.innerHTML = "";
      cards.forEach(card => board.appendChild(card));
    }

    function speak(text) {
      if (!("speechSynthesis" in window)) return;
      window.speechSynthesis.cancel();
      const msg = new SpeechSynthesisUtterance(text);
      msg.lang = "ru-RU";
      msg.rate = 0.96;
      msg.pitch = 1.2;
      window.speechSynthesis.speak(msg);
    }

    function unlockSite() {
      const value = pinInput.value.trim();
      if (value !== correctPin) {
        pinInput.value = "";
        pinInput.placeholder = "Неверный код";
        pinInput.style.borderColor = "rgba(255, 87, 87, 0.7)";
        setTimeout(() => {
          pinInput.style.borderColor = "rgba(143, 123, 255, 0.25)";
          pinInput.placeholder = "Код";
        }, 800);
        return;
      }

      entryOverlay.classList.add("hidden");
      app.style.display = "block";
      setTimeout(() => {
        speak("С днём рождения, Миша! Твоя сюрприз-приглашение уже ждёт тебя!");
      }, 300);
    }

    function openChallenge(key) {
      const challenge = challenges[key];
      activeChallengeKey = key;

      modalTitle.textContent = challenge.title;
      modalQuestion.textContent = challenge.question;
      modalInput.value = "";
      modalInput.focus();
      modal.classList.add("show");
    }

    function closeChallenge() {
      modal.classList.remove("show");
      modalInput.value = "";
      activeChallengeKey = null;
    }

    document.querySelectorAll(".step-card").forEach(card => {
      const btn = card.querySelector(".section-btn");
      btn.addEventListener("click", () => {
        const key = card.dataset.key;
        if (!card.classList.contains("locked")) return;
        openChallenge(key);
      });
    });

    modalBtn.addEventListener("click", () => {
      if (!activeChallengeKey) return;
      const challenge = challenges[activeChallengeKey];
      const answer = modalInput.value.trim().toLowerCase();

      if (answer === challenge.answer) {
        const card = document.querySelector(`.step-card[data-key="${activeChallengeKey}"]`);
        card.classList.remove("locked");
        card.classList.add("open");
        closeChallenge();
        if (activeChallengeKey === "surprise") {
          speak("Главный сюрприз открыт! Ты особенный, Миша. Пусть каждый день приносит тебе чудо!");
        } else {
          speak("Ура! Следующий шаг открыт.");
        }
      } else {
        modalInput.value = "";
        modalInput.placeholder = "Попробуй ещё раз";
        modalInput.style.borderColor = "rgba(255, 87, 87, 0.7)";
        setTimeout(() => {
          modalInput.style.borderColor = "rgba(143, 123, 255, 0.2)";
          modalInput.placeholder = "Напиши ответ";
        }, 800);
      }
    });

    modalInput.addEventListener("keydown", (event) => {
      if (event.key === "Enter") modalBtn.click();
    });

    unlockBtn.addEventListener("click", unlockSite);
    pinInput.addEventListener("keydown", (event) => {
      if (event.key === "Enter") unlockSite();
    });

    initMemoryBoard();
  </script>
</body>
</html>