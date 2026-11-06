
const $ = (id) => document.getElementById(id);

const getData = () => {
  try {
    return JSON.parse(localStorage.getItem("littleUsData")) || {};
  } catch {
    return {};
  }
};

let data = getData();

function save() {
  localStorage.setItem("littleUsData", JSON.stringify(data));
}

function showScreen(id) {
  document.querySelectorAll(".screen").forEach(screen => {
    screen.classList.add("hidden");
  });

  $(id)?.classList.remove("hidden");

  const loggedIn = id !== "loginScreen";
  $("bottomNav").classList.toggle("hidden", !loggedIn);

  if (loggedIn) render();
}

function render() {
  $("growthDays").textContent = data.days || 0;
  $("coinCount").textContent = `${data.coins || 0} love coins`;
  $("treeDesign").textContent =
    ["🌱", "🌿", "🪴", "🌳"][
      Math.min(3, Math.floor((data.days || 0) / 8))
    ];

  $("treeMessage").textContent =
    data.days >= 30
      ? "our little love has blossomed ♡"
      : "every little moment helps us grow.";

  $("dailyQuestion").textContent = getQuestion();
  $("questionText").textContent = getQuestion();
  $("questionNumber").textContent = `day ${(data.days || 0) + 1} of 30`;

  renderShop();
  renderInbox();
  renderGifts();
  renderSongs();
  renderPlaylists();
  renderSurprises();
}

const questions = [
  "What's one little thing that makes you feel loved?",
  "What's your favourite memory of us?",
  "What is something you want us to experience together?",
  "What was your first impression of me?",
  "Which song reminds you of us?",
  "What little thing about me makes you smile?",
  "What's your dream date with me?",
  "What is something you want me to know?",
  "What's your favourite thing about our relationship?",
  "If we could travel anywhere together, where would we go?"
];

function getQuestion() {
  return questions[(data.days || 0) % questions.length];
}

// Demo login
$("signupBtn").addEventListener("click", () => {
  const email = $("email").value.trim();
  const password = $("password").value;

  if (!email || password.length < 6) {
    alert("Enter an email and a password with at least 6 characters.");
    return;
  }

  data.email = email;
  data.days = 0;
  data.coins = 0;
  data.answers = [];
  data.gifts = [];
  data.songs = [];
  data.playlists = [];
  data.prompts = [];
  data.preferences = {};
  data.inviteCode = Math.random().toString(36).slice(2, 8).toUpperCase();

  save();
  alert("Your demo account is ready! Your invite code is " + data.inviteCode);
  showScreen("gardenScreen");
});

$("loginBtn").addEventListener("click", () => {
  if (!data.email) {
    alert("Create your demo account first.");
    return;
  }

  if ($("email").value.trim() !== data.email) {
    alert("This demo only recognises the email used to create it.");
    return;
  }

  showScreen("gardenScreen");
});

$("joinBtn").addEventListener("click", () => {
  alert("Partner connection will be activated when we set up Supabase.");
});

// Navigation
document.querySelectorAll("[data-screen]").forEach(button => {
  button.addEventListener("click", () => {
    showScreen(button.dataset.screen);
  });
});

// Daily answers
$("submitAnswerBtn").addEventListener("click", () => {
  const answer = $("answerText").value.trim();

  if (!answer) {
    alert("Write a little answer first ♡");
    return;
  }

  if (data.answers?.some(a => a.day === (data.days || 0) + 1)) {
    alert("You've already answered today's question!");
    return;
  }

  data.answers = data.answers || [];
  data.answers.push({
    day: (data.days || 0) + 1,
    question: getQuestion(),
    answer
  });

  // Demo mode: one answer advances the local preview.
  data.days = Math.min(30, (data.days || 0) + 1);
  data.coins = (data.coins || 0) + 10;

  $("answerText").value = "";
  save();
  render();

  alert("Your answer has been saved! You earned 10 love coins. 💗");
});

// Rewards
const rewards = [
  { name: "Pink flowers", icon: "🌷", price: 10 },
  { name: "Little mushroom", icon: "🍄", price: 20 },
  { name: "Garden butterflies", icon: "🦋", price: 30 },
  { name: "Cute teddy bear", icon: "🧸", price: 40 },
  { name: "Fairy lights", icon: "✨", price: 50 },
  { name: "Little birdhouse", icon: "🐦", price: 60 }
];

function renderShop() {
  $("rewardShop").innerHTML = rewards.map((item, index) => `
    <div class="panel">
      <h3>${item.icon} ${item.name}</h3>
      <p>${item.price} love coins</p>
      <button onclick="buyReward(${index})">Add to our garden</button>
    </div>
  `).join("");
}

window.buyReward = function(index) {
  const item = rewards[index];

  if ((data.coins || 0) < item.price) {
    alert("You need more love coins!");
    return;
  }

  data.coins -= item.price;
  data.decorations = data.decorations || [];
  data.decorations.push(item.icon);
  save();
  render();

  alert(`${item.name} added to your garden!`);
};

// Mood check-in
$("saveMoodBtn").addEventListener("click", () => {
  const mood = $("mood").value;
  const note = $("moodNote").value.trim();

  data.checkins = data.checkins || [];
  data.checkins.push({
    mood,
    note,
    shared: $("shareMood").checked,
    date: new Date().toISOString()
  });

  if ($("shareMood").checked) {
    data.prompts = data.prompts || [];
    data.prompts.push({
      title: "A little update from your person",
      body: `Your person is feeling ${mood.toLowerCase()}. Maybe send them a little love? 💗`
    });
  }

  save();
  $("moodNote").value = "";
  alert("Your check-in has been saved ♡");
  renderInbox();
});

// Favourite things
$("savePreferencesBtn").addEventListener("click", () => {
  data.preferences = {
    flower: $("favouriteFlower").value.trim(),
    chocolate: $("favouriteChocolate").value.trim(),
    toy: $("favouriteToy").value.trim(),
    snack: $("favouriteSnack").value.trim(),
    gift: $("favouriteGift").value.trim()
  };

  save();
  alert("Your little favourites have been saved! 🌷");
  renderSurprises();
});

function renderSurprises() {
  const p = data.preferences || {};
  const items = [
    ["Flowers", p.flower],
    ["Chocolates", p.chocolate],
    ["Toys", p.toy],
    ["Snacks", p.snack],
    ["Other favourites", p.gift]
  ].filter(item => item[1]);

  $("surpriseSuggestions").innerHTML = items.length
    ? items.map(item => `
      <div class="panel">
        <strong>${item[0]}:</strong> ${escapeHTML(item[1])}
        <p>Remember this little favourite for a future surprise ♡</p>
      </div>
    `).join("")
    : "<p>Add your favourite things above to get started!</p>";
}

function renderInbox() {
  $("loveInbox").innerHTML = (data.prompts || []).length
    ? data.prompts.map(item => `
      <div class="panel">
        <h3>${escapeHTML(item.title)}</h3>
        <p>${escapeHTML(item.body)}</p>
      </div>
    `).join("")
    : "<p>No little messages yet ♡</p>";
}

// Personalised scrapbook gifts
$("createGiftBtn").addEventListener("click", () => {
  const title = $("giftTitle").value.trim();
  const letter = $("giftLetter").value.trim();

  if (!title || !letter) {
    alert("Add a title and a little letter first.");
    return;
  }

  const files = [...$("giftPhotos").files];

  data.gifts = data.gifts || [];
  data.gifts.push({
    title,
    letter,
    sticker: $("giftSticker").value,
    photoNames: files.map(file => file.name),
    date: new Date().toISOString()
  });

  save();
  $("giftTitle").value = "";
  $("giftLetter").value = "";
  $("giftPhotos").value = "";

  alert("Your little gift has been created! 💌");
  renderGifts();
});

function renderGifts() {
  $("receivedGifts").innerHTML = (data.gifts || []).map((gift, index) => `
    <div class="panel">
      <h3>${escapeHTML(gift.sticker)} ${escapeHTML(gift.title)}</h3>
      <p>${escapeHTML(gift.letter)}</p>
      <small>${gift.photoNames.length} photo(s) selected</small>
      <button onclick="openGift(${index})">Open gift 💗</button>
    </div>
  `).join("") || "<p>Your little gifts will appear here ♡</p>";
}

window.openGift = function(index) {
  const gift = data.gifts[index];

  alert(
    `${gift.sticker}\n\n${gift.title}\n\n${gift.letter}\n\nMade with love ♡`
  );
};

// Song dedications
$("sendSongBtn").addEventListener("click", () => {
  const name = $("songName").value.trim();
  const link = $("songLink").value.trim();

  if (!name || !link) {
    alert("Add a song name and its music link.");
    return;
  }

  if (!isSafeURL(link)) {
    alert("Please enter a valid Spotify or music link.");
    return;
  }

  data.songs = data.songs || [];
  data.songs.push({
    name,
    artist: $("songArtist").value.trim(),
    link,
    letter: $("songLetter").value.trim()
  });

  save();
  renderSongs();
  alert("Your song dedication is saved! 🎧");
});

function renderSongs() {
  $("songGifts").innerHTML = (data.songs || []).map(song => `
    <div class="panel">
      <h3>🎧 ${escapeHTML(song.name)}</h3>
      <p>${escapeHTML(song.artist)}</p>
      <p>${escapeHTML(song.letter)}</p>
      <a href="${escapeHTML(song.link)}" target="_blank" rel="noopener">Open song</a>
    </div>
  `).join("");
}

// Spotify playlist gifts
$("sendPlaylistBtn").addEventListener("click", () => {
  const title = $("playlistTitle").value.trim();
  const link = $("playlistLink").value.trim();

  if (!title || !link) {
    alert("Add a playlist name and Spotify link.");
    return;
  }

  if (!isSafeURL(link) || !/spotify\.com/i.test(new URL(link).hostname)) {
    alert("Please paste a valid Spotify playlist link.");
    return;
  }

  data.playlists = data.playlists || [];
  data.playlists.push({
    title,
    link,
    message: $("playlistMessage").value.trim()
  });

  save();
  renderPlaylists();
  alert("Your playlist gift is ready! 🎀");
});

function renderPlaylists() {
  $("playlistGifts").innerHTML = (data.playlists || []).map(playlist => `
    <div class="panel">
      <h3>🎀 ${escapeHTML(playlist.title)}</h3>
      <p>${escapeHTML(playlist.message)}</p>
      <a href="${escapeHTML(playlist.link)}" target="_blank" rel="noopener">Listen on Spotify</a>
    </div>
  `).join("");
}

// Certificate
if ((data.days || 0) >= 30) {
  $("certificateEvidence").textContent =
    "30 little steps, all those answers, and a garden full of memories. ♡";
}

function isSafeURL(value) {
  try {
    const url = new URL(value);
    return url.protocol === "https:" || url.protocol === "http:";
  } catch {
    return false;
  }
}

function escapeHTML(value) {
  return String(value).replace(/[&<>"']/g, char => ({
    "&": "&amp;",
    "<": "&lt;",
    ">": "&gt;",
    '"': "&quot;",
    "'": "&#39;"
  })[char]);
}

// Start
showScreen("loginScreen");
