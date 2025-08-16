import { useState, useEffect } from "react"; import Confetti from "react-confetti";

function QuizGame({ onScore }) { const questions = [ { q: "Quanto é 5 + 7?", options: ["10", "12", "14"], answer: "12" }, { q: "Qual a capital do Brasil?", options: ["Rio de Janeiro", "Brasília", "São Paulo"], answer: "Brasília" }, ];

const [current, setCurrent] = useState(0); const [score, setScore] = useState(0);

const handleAnswer = (opt) => { if (opt === questions[current].answer) { setScore(score + 1); onScore(1); } if (current + 1 < questions.length) setCurrent(current + 1); else alert(Fim do jogo! Você acertou ${score + (opt === questions[current].answer ? 1 : 0)} de ${questions.length}); };

return ( <div className="p-6 bg-white rounded-2xl shadow w-full md:w-2/3 mx-auto"> <h2 className="text-2xl font-bold mb-4">{questions[current].q}</h2> <div className="grid grid-cols-2 gap-4"> {questions[current].options.map((opt, i) => ( <button key={i} onClick={() => handleAnswer(opt)} className="bg-indigo-100 text-indigo-800 py-2 rounded-lg hover:bg-indigo-200 transition" > {opt} </button> ))} </div> </div> ); }

function MemoryGame({ onScore }) { const cardsArray = ["🍎", "🍌", "🍇", "🍎", "🍌", "🍇"]; const [cards, setCards] = useState(cardsArray.sort(() => Math.random() - 0.5)); const [flipped, setFlipped] = useState([]); const [matched, setMatched] = useState([]);

const handleFlip = (index) => { if (flipped.length === 2 || flipped.includes(index)) return; const newFlipped = [...flipped, index]; setFlipped(newFlipped);

if (newFlipped.length === 2) {
  const [first, second] = newFlipped;
  if (cards[first] === cards[second]) {
    setMatched([...matched, cards[first]]);
    onScore(1);
  }
  setTimeout(() => setFlipped([]), 1000);
}

};

return ( <div className="grid grid-cols-3 gap-4 p-6 bg-white rounded-2xl shadow w-full md:w-2/3 mx-auto"> {cards.map((card, index) => ( <button key={index} onClick={() => handleFlip(index)} className="h-20 flex items-center justify-center text-2xl bg-indigo-100 rounded-lg hover:bg-indigo-200" > {flipped.includes(index) || matched.includes(card) ? card : "❓"} </button> ))} </div> ); }

export default function App() { const [page, setPage] = useState("home"); const [totalScore, setTotalScore] = useState(0); const [playerName, setPlayerName] = useState(""); const [playerAvatar, setPlayerAvatar] = useState("/avatars/warrior.png"); const [ranking, setRanking] = useState([]); const [showConfetti, setShowConfetti] = useState(false);

const getRank = (score) => { if (score < 10) return "Bronze"; if (score < 20) return "Prata"; if (score < 30) return "Ouro"; if (score < 40) return "Diamante"; if (score < 50) return "Mestre"; if (score < 60) return "Desafiante"; return "Pro"; };

const addScore = (points) => { const oldRank = getRank(totalScore); const newScore = totalScore + points; const newRank = getRank(newScore); setTotalScore(newScore);

if (newRank !== oldRank) {
  setShowConfetti(true);
  setTimeout(() => setShowConfetti(false), 4000);
}

};

const saveScore = () => { if (!playerName) { alert("Digite seu nome para salvar no ranking!"); return; } const newRanking = [...ranking, { name: playerName, avatar: playerAvatar, score: totalScore }]; newRanking.sort((a, b) => b.score - a.score); setRanking(newRanking); setTotalScore(0); alert("Pontuação salva!"); };

const avatars = [ "/avatars/warrior.png", "/avatars/mage.png", "/avatars/robot.png", "/avatars/fox.png", "/avatars/dragon.png", "/avatars/crown.png" ];

const nextRank = (score) => { if (score < 10) return { label: "Prata", needed: 10 }; if (score < 20) return { label: "Ouro", needed: 20 }; if (score < 30) return { label: "Diamante", needed: 30 }; if (score < 40) return { label: "Mestre", needed: 40 }; if (score < 50) return { label: "Desafiante", needed: 50 }; if (score < 60) return { label: "Pro", needed: 60 }; return { label: "Max", needed: score }; };

const progress = () => { const next = nextRank(totalScore); const currentBase = next.needed - 10; const percent = ((totalScore - currentBase) / (next.needed - currentBase)) * 100; return Math.min(percent, 100); };

return ( <div className="min-h-screen bg-gray-100"> {showConfetti && <Confetti />}

{/* Navbar */}
  <nav className="bg-white shadow p-4 flex justify-between items-center">
    <h1
      className="text-2xl font-bold text-indigo-600 cursor-pointer"
      onClick={() => setPage("home")}
    >
      GameSite
    </h1>
    <ul className="flex space-x-6">
      <li><button onClick={() => setPage("quiz")} className="hover:text-indigo-600">Quiz</button></li>
      <li><button onClick={() => setPage("memory")} className="hover:text-indigo-600">Memória</button></li>
      <li><button onClick={() => setPage("ranking")} className="hover:text-indigo-600">Ranking</button></li>
    </ul>
    <div className="font-semibold text-indigo-600">⭐ Pontos: {totalScore} | {getRank(totalScore)}</div>
  </nav>

  {/* Home */}
  {page === "home" && (
    <header className="text-center py-16 bg-gradient-to-r from-indigo-500 to-blue-500 text-white">
      <h2 className="text-4xl font-bold mb-4">🎮 Bem-vindo à Plataforma de Jogos</h2>
      <p className="text-lg max-w-xl mx-auto">
        Escolha um jogo no menu e divirta-se aprendendo!
      </p>
      <div className="mt-6 flex flex-col items-center gap-4">
        <input
          type="text"
          placeholder="Digite seu nome"
          value={playerName}
          onChange={(e) => setPlayerName(e.target.value)}
          className="px-4 py-2 rounded-lg text-gray-800"
        />
        <div className="flex gap-3">
          {avatars.map((a, i) => (
            <img
              key={i}
              src={a}
              alt="avatar"
              className={`w-14 h-14 rounded-full border-4 cursor-pointer ${playerAvatar === a ? "border-yellow-400" : "border-transparent"}`}
              onClick={() => setPlayerAvatar(a)}
            />
          ))}
        </div>
        <button
          onClick={saveScore}
          className="bg-white text-indigo-600 font-semibold px-4 py-2 rounded-lg hover:bg-gray-200"
        >
          Salvar Pontos
        </button>

        {/* Barra de progresso */}
        <div className="w-64 bg-gray-300 rounded-full h-4 mt-4">
          <div className="bg-green-500 h-4 rounded-full" style={{ width: `${progress()}%` }}></div>
        </div>
        <p className="mt-2 text-sm">Rumo ao nível {nextRank(totalScore).label}!</p>
      </div>
    </header>
  )}

  {/* Quiz */}
  {page === "quiz" && <QuizGame onScore={addScore} />}

  {/* Memory Game */}
  {page === "memory" && <MemoryGame onScore={addScore} />}

  {/* Ranking */}
  {page === "ranking" && (
    <section className="max-w-2xl mx-auto py-12 px-4">
      <h2 className="text-3xl font-bold text-center mb-8 text-indigo-600">🏆 Ranking de Jogadores</h2>
      <ul className="bg-white rounded-2xl shadow divide-y">
        {ranking.length === 0 && (
          <p className="text-center p-4 text-gray-500">Nenhum jogador no ranking ainda.</p>
        )}
        {ranking.map((r, i) => (
          <li key={i} className="p-4 flex items-center justify-between">
            <div className="flex items-center gap-3">
              <img src={r.avatar} alt="avatar" className="w-10 h-10 rounded-full border" />
              <span>{i + 1}. {r.name} ({getRank(r.score)})</span>
            </div>
            <span className="font-semibold text-indigo-600">{r.score} pts</span>
          </li>
        ))}
      </ul>
    </section>
  )}

  {/* Footer */}
  <footer className="bg-white text-center p-4 shadow mt-8">
    <p className="text-gray-600">© 2025 GameSite - Diversão e Aprendizado</p>
  </footer>
</div>

); }
