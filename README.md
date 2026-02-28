# Solu-o-financeira-

# Backend (Python)

Para o backend, você pode usar Flask (ou FastAPI) para criar uma API.

```bash
# Instale Flask com pip
pip install Flask
```

app.py

```python
from flask import Flask, request, jsonify
from flask_cors import CORS

app = Flask(__name__)
CORS(app)

# Rota para responder perguntas frequentes
@app.route('/api/faq', methods=['POST'])
def faq():
    user_question = request.json.get('question')
    # Simular uma resposta de IA
    response = {"answer": "Isso é um exemplo de resposta para: " + user_question}
    return jsonify(response)

# Rota para cálculos financeiros
@app.route('/api/calculo', methods=['POST'])
def calcular():
    valor = request.json.get('valor')
    taxa = request.json.get('taxa')
    tempo = request.json.get('tempo')
    
    # Cálculo simples de juros simples
    juros = valor * (taxa / 100) * tempo
    return jsonify({"juros": juros})

if __name__ == '__main__':
    app.run(debug=True)
```

# Frontend (HTML/CSS/JavaScript)

Você pode usar HTML e JavaScript (com fetch API) para criar a interface do usuário.

index.html

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interação Financeira</title>
    <style>
        body { font-family: Arial, sans-serif; }
        #resultado { margin-top: 20px; }
    </style>
</head>
<body>
    <h1>Assistente Financeiro IA</h1>
    
    <div>
        <h2>Pergunte-me sobre finanças</h2>
        <input type="text" id="pergunta" placeholder="Digite sua pergunta">
        <button onclick="enviarPergunta()">Enviar</button>
    </div>
    
    <div id="resultado"></div>

    <h2>Calculo Financeiro</h2>
    <input type="number" id="valor" placeholder="Valor">
    <input type="number" id="taxa" placeholder="Taxa de Juros">
    <input type="number" id="tempo" placeholder="Tempo (em anos)">
    <button onclick="calcularJuros()">Calcular Juros</button>
    
    <script>
        function enviarPergunta() {
            const pergunta = document.getElementById('pergunta').value;
            fetch('http://localhost:5000/api/faq', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ question: pergunta })
            })
            .then(response => response.json())
            .then(data => {
                document.getElementById('resultado').innerText = data.answer;
            });
        }

        function calcularJuros() {
            const valor = document.getElementById('valor').value;
            const taxa = document.getElementById('taxa').value;
            const tempo = document.getElementById('tempo').value;
            fetch('http://localhost:5000/api/calculo', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ valor: parseFloat(valor), taxa: parseFloat(taxa), tempo: parseFloat(tempo) })
            })
            .then(response => response.json())
            .then(data => {
                document.getElementById('resultado').innerText = "Juros: " + data.juros;
            });
        }
    </script>
</body>
</html>
```

# Inicializando a Aplicação

1. Backend: Execute `python app.py` para iniciar o servidor Flask.
2. Frontend: Abra o `index.html` em um navegador.

# Observações

- As respostas das FAQs são simuladas. Para modelagem real com IA, considere integrar uma API de IA como OpenAI.
- As interações financeiras são simplificadas; você pode expandir com validações e cálculos mais complexos.
- Certifique-se de configurar CORS e de usar HTTPS em produção.
