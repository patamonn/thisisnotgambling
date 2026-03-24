<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Team Betting Game (Event Ready)</title>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore-compat.js"></script>
  <style>
    body { font-family: Arial; text-align: center; padding: 20px; }
    .option { margin: 10px; padding: 15px; border: 1px solid #ccc; cursor: pointer; }
    .selected { background-color: #d0f0ff; }
    .disabled { opacity: 0.5; pointer-events: none; }
    button { margin: 5px; padding: 10px 15px; }
  </style>
</head>
<body>
  <h1>🎲 Team Betting Game</h1>

  <input id="username" placeholder="Enter your name" />
  <button onclick="joinGame()">Join</button>

  <h3>Tokens: <span id="tokens">0</span></h3>

  <h2 id="roundState">State: OPEN</h2>

  <div id="options"></div>

  <input type="number" id="betAmount" placeholder="Bet amount" />
  <br>
  <button onclick="placeBet()">Place Bet</button>

  <h3>📊 Live Bets</h3>
  <div id="stats"></div>

  <h2>🏆 Leaderboard</h2>
  <ul id="leaderboard"></ul>

  <h2>🎮 Host Panel</h2>
  <button onclick="setState('OPEN')">Open Betting</button>
  <button onclick="setState('CLOSED')">Close Betting</button>
  <button onclick="resolveRoundPrompt()">Resolve Round</button>

  <h3 id="message"></h3>

  <script>
    const firebaseConfig = {
      apiKey: "YOUR_KEY",
      authDomain: "YOUR_DOMAIN",
      projectId: "YOUR_PROJECT_ID"
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    let user = null;
    let selectedOption = null;

    const options = ["A", "B", "C", "D"];

    const optionsDiv = document.getElementById("options");

    options.forEach((opt, i) => {
      const div = document.createElement("div");
      div.className = "option";
      div.innerText = "Option " + opt;
      div.onclick = () => selectOption(i, div);
      optionsDiv.appendChild(div);
    });

    function joinGame() {
      const name = document.getElementById("username").value.trim();
      if (!name) return alert("Enter a name");

      user = name;

      db.collection("players").doc(user).set({
        tokens: 100
      }, { merge: true });

      listenLeaderboard();
      listenStats();
      listenState();
    }

    function selectOption(i, el) {
      selectedOption = i;
      document.querySelectorAll(".option").forEach(e => e.classList.remove("selected"));
      el.classList.add("selected");
    }

    async function placeBet() {
      const stateDoc = await db.collection("game").doc("state").get();
      if (stateDoc.data()?.status !== "OPEN") {
        return alert("Betting is closed!");
      }

      const bet = parseInt(document.getElementById("betAmount").value);
      if (!user || selectedOption === null || !bet) return;

      const ref = db.collection("players").doc(user);
      const player = await ref.get();
      const tokens = player.data().tokens;

      if (bet > tokens) return alert("Not enough tokens");

      await ref.update({ tokens: tokens - bet });

      await db.collection("bets").add({ user, option: selectedOption, amount: bet });

      document.getElementById("message").innerText = "Bet placed!";
    }

    function listenLeaderboard() {
      db.collection("players").onSnapshot(snapshot => {
        const list = document.getElementById("leaderboard");
        list.innerHTML = "";

        snapshot.docs
          .map(doc => ({ name: doc.id, ...doc.data() }))
          .sort((a, b) => b.tokens - a.tokens)
          .forEach((p, i) => {
            const li = document.createElement("li");
            const medal = i === 0 ? "🥇" : i === 1 ? "🥈" : i === 2 ? "🥉" : "";
            li.innerText = `${medal} ${p.name}: ${p.tokens}`;
            list.appendChild(li);
          });
      });
    }

    function listenStats() {
      db.collection("bets").onSnapshot(snapshot => {
        let totals = [0,0,0,0];
        let counts = [0,0,0,0];

        snapshot.forEach(doc => {
          const b = doc.data();
          totals[b.option] += b.amount;
          counts[b.option]++;
        });

        const statsDiv = document.getElementById("stats");
        statsDiv.innerHTML = "";

        totals.forEach((t, i) => {
          const div = document.createElement("div");
          div.innerText = `Option ${options[i]} — ${t} tokens (${counts[i]} players)`;
          statsDiv.appendChild(div);
        });
      });
    }

    function listenState() {
      db.collection("game").doc("state").onSnapshot(doc => {
        const state = doc.data()?.status || "OPEN";
        document.getElementById("roundState").innerText = "State: " + state;
      });
    }

    function setState(state) {
      db.collection("game").doc("state").set({ status: state });
    }

    async function resolveRoundPrompt() {
      const winningOption = parseInt(prompt("Enter winning option (0=A,1=B,2=C,3=D)"));
      if (isNaN(winningOption)) return;

      const betsSnap = await db.collection("bets").get();
      let totalPool = 0;
      let winners = [];

      betsSnap.forEach(doc => {
        const bet = doc.data();
        totalPool += bet.amount;
        if (bet.option === winningOption) winners.push(bet);
      });

      const totalWinning = winners.reduce((sum, w) => sum + w.amount, 0);

      for (let w of winners) {
        const share = (w.amount / totalWinning) * totalPool;
        const ref = db.collection("players").doc(w.user);
        const player = await ref.get();
        await ref.update({ tokens: player.data().tokens + share });
      }

      const batch = db.batch();
      betsSnap.forEach(doc => batch.delete(doc.ref));
      await batch.commit();

      setState("OPEN");
      alert("Round resolved!");
    }
  </script>
</body>
</html>
