import { useState } from 'react';
import { Volume2, RotateCcw } from 'lucide-react';

export default function NumberGuessingGame() {
  const [numeroSecreto] = useState(Math.floor(Math.random() * 100) + 1);
  const [palpite, setPalpite] = useState('');
  const [tentativas, setTentativas] = useState([]);
  const [mensagem, setMensagem] = useState('');
  const [gameOver, setGameOver] = useState(false);
  const [ganhou, setGanhou] = useState(false);

  const tentativasRestantes = 5 - tentativas.length;

  const fazer_palpite = () => {
    const num = parseInt(palpite);

    if (!palpite || num < 1 || num > 100) {
      setMensagem('⚠️ Digite um número entre 1 e 100!');
      return;
    }

    if (tentativas.includes(num)) {
      setMensagem('🔄 Você já tentou esse número!');
      return;
    }

    const novasTentativas = [...tentativas, num];
    setTentativas(novasTentativas);
    setPalpite('');

    if (num === numeroSecreto) {
      setMensagem('🎉 Parabéns! Você acertou!');
      setGanhou(true);
      setGameOver(true);
    } else if (num < numeroSecreto) {
      setMensagem(`📈 O número secreto é MAIOR que ${num}`);
    } else {
      setMensagem(`📉 O número secreto é MENOR que ${num}`);
    }

    if (novasTentativas.length === 5 && num !== numeroSecreto) {
      setMensagem(`😢 Você perdeu! O número era ${numeroSecreto}`);
      setGameOver(true);
    }
  };

  const reiniciar = () => {
    window.location.reload();
  };

  const handleKeyPress = (e) => {
    if (e.key === 'Enter' && !gameOver) {
      fazer_palpite();
    }
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-600 via-purple-600 to-pink-500 flex items-center justify-center p-4">
      <div className="w-full max-w-md">
        {/* Cabeçalho */}
        <div className="text-center mb-8">
          <h1 className="text-5xl font-bold text-white mb-2">🎯 Adivinhe o Número</h1>
          <p className="text-blue-100 text-lg">Tente descobrir o número secreto em até 5 tentativas</p>
        </div>

        {/* Card principal */}
        <div className="bg-white rounded-2xl shadow-2xl p-8 mb-6">
          {/* Indicador de tentativas */}
          <div className="mb-8">
            <div className="flex justify-between items-center mb-3">
              <span className="text-gray-700 font-semibold">Tentativas</span>
              <span className="text-2xl font-bold text-purple-600">{tentativasRestantes}/5</span>
            </div>
            <div className="w-full bg-gray-200 rounded-full h-3 overflow-hidden">
              <div
                className={`h-full transition-all duration-300 rounded-full ${
                  tentativasRestantes > 3 ? 'bg-green-500' : tentativasRestantes > 1 ? 'bg-yellow-500' : 'bg-red-500'
                }`}
                style={{ width: `${((5 - tentativasRestantes) / 5) * 100}%` }}
              />
            </div>
          </div>

          {/* Input de palpite */}
          {!gameOver && (
            <div className="mb-6">
              <label className="block text-gray-700 font-semibold mb-3">Seu palpite (1-100)</label>
              <div className="flex gap-2">
                <input
                  type="number"
                  value={palpite}
                  onChange={(e) => setPalpite(e.target.value)}
                  onKeyPress={handleKeyPress}
                  placeholder="Digite um número..."
                  min="1"
                  max="100"
                  className="flex-1 px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none text-lg font-semibold"
                  autoFocus
                />
                <button
                  onClick={fazer_palpite}
                  className="bg-purple-600 hover:bg-purple-700 text-white px-6 py-3 rounded-lg font-bold transition-colors"
                >
                  Enviar
                </button>
              </div>
            </div>
          )}

          {/* Mensagem de feedback */}
          {mensagem && (
            <div
              className={`p-4 rounded-lg mb-6 text-center font-semibold text-lg ${
                ganhou
                  ? 'bg-green-100 text-green-700 border-2 border-green-300'
                  : gameOver
                  ? 'bg-red-100 text-red-700 border-2 border-red-300'
                  : 'bg-blue-100 text-blue-700 border-2 border-blue-300'
              }`}
            >
              {mensagem}
            </div>
          )}

          {/* Histórico de tentativas */}
          {tentativas.length > 0 && (
            <div className="mb-6">
              <h3 className="text-gray-700 font-semibold mb-3">Seus palpites:</h3>
              <div className="grid grid-cols-5 gap-2">
                {Array.from({ length: 5 }).map((_, i) => (
                  <div
                    key={i}
                    className={`p-3 rounded-lg text-center font-bold text-lg transition-all ${
                      i < tentativas.length
                        ? 'bg-purple-600 text-white'
                        : 'bg-gray-200 text-gray-400'
                    }`}
                  >
                    {i < tentativas.length ? tentativas[i] : '-'}
                  </div>
                ))}
              </div>
            </div>
          )}

          {/* Botão de reiniciar */}
          {gameOver && (
            <button
              onClick={reiniciar}
              className="w-full bg-blue-600 hover:bg-blue-700 text-white py-3 rounded-lg font-bold text-lg flex items-center justify-center gap-2 transition-colors"
            >
              <RotateCcw size={20} />
              Jogar Novamente
            </button>
          )}
        </div>

        {/* Dica */}
        <div className="text-center text-blue-100 text-sm">
          <p>💡 Dica: A cada tentativa, você recebe uma dica para se aproximar do número!</p>
        </div>
      </div>
    </div>
  );
}# Projeto-Terceiro-trimestre-Programa-o.
