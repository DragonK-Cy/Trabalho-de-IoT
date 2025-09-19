# Trabalho-de-IoT
Trabalho da Disciplina de: Internet das Coisas
Me chamo: Felipe Jorge Cavalcante Oliveira
Curso: Ciência da Computação
8° Sementre


Bom dia, boa tarde e boa noite
Primeiramente este é somente um trabalho simples, para o curso de: Ciência da Computação, da disciplina de internet das coisas.

1° Será preciso utilizar o: VSCode
2° Será necessário abrir o: VSCode
3° Instale os seguintes modulos: 'pip install flask fpdf' , 'pip install flask' e tenha o python instalado.

Após isso irei apresentar ás versões do projeto:

1° Versão: 
------------------------------------------------------------------------------------------------------------------------|
Contéudo: 
📍 Versão 1.0 - Sistema Básico de Comunicação
Data: Desenvolvimento inicial
Funcionalidades:

✅ Comunicação TCP entre cliente e servidor

✅ Simulação de sensor de temperatura

✅ Interface terminal simples

✅ Timestamp básico das leituras
------------------------------------------------------------------------------------------------------------------------|
Código:

from flask import Flask, render_template_string
import random
import threading
import time
from datetime import datetime

# Cria o servidor web
app = Flask(__name__)

# Dados iniciais dos sensores (armazenados em memória)
dados_sensores = {
    "temperatura": 25.0,
    "nivel_agua": 50,
    "movimento": "Não",
    "acesso": "Trancado"
}

# Gera dados aleatórios para os sensores
def gerar_dados_sensores():
    while True:
        dados_sensores['temperatura'] = round(random.uniform(20.0, 35.0), 1)
        dados_sensores['nivel_agua'] = random.randint(0, 100)
        dados_sensores['movimento'] = "Sim" if random.choice([True, False]) else "Não"
        dados_sensores['acesso'] = "Liberado" if random.choice([True, False]) else "Trancado"
        
        # Espera 3 segundos antes de gerar novos dados
        time.sleep(3)

# Página HTML que será exibida (tudo em uma string para simplicidade)
HTML_PAGE = """
<!DOCTYPE html>
<html>
<head>
    <title>Controle de Horta IoT</title>
    <meta http-equiv="refresh" content="3"> <!-- Atualiza a página a cada 3 segundos -->
    <style>
        body { font-family: Arial, sans-serif; margin: 40px; background-color: #f0f0f0; }
        h1 { color: #2c3e50; text-align: center; }
        .sensor { 
            background: white; 
            padding: 20px; 
            margin: 10px; 
            border-radius: 10px; 
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            text-align: center;
        }
        .valor { 
            font-size: 24px; 
            font-weight: bold; 
            color: #3498db; 
        }
        .timestamp { 
            color: #7f8c8d; 
            font-size: 14px; 
        }
    </style>
</head>
<body>
    <h1>🌿 Monitoramento da Horta IoT</h1>
    
    <div class="sensor">
        <h2>🌡️ Temperatura</h2>
        <div class="valor">{{ temperatura }}°C</div>
    </div>
    
    <div class="sensor">
        <h2>💧 Nível da Água</h2>
        <div class="valor">{{ nivel_agua }}%</div>
    </div>
    
    <div class="sensor">
        <h2>🚶 Movimento Detectado</h2>
        <div class="valor">{{ movimento }}</div>
    </div>
    
    <div class="sensor">
        <h2>🔐 Controle de Acesso</h2>
        <div class="valor">{{ acesso }}</div>
    </div>
    
    <div class="timestamp">
        <p>Última atualização: {{ timestamp }}</p>
    </div>
</body>
</html>
"""

# Rota principal que renderiza a página com os dados
@app.route('/')
def index():
    return render_template_string(HTML_PAGE, 
                                temperatura=dados_sensores['temperatura'],
                                nivel_agua=dados_sensores['nivel_agua'],
                                movimento=dados_sensores['movimento'],
                                acesso=dados_sensores['acesso'],
                                timestamp=datetime.now().strftime("%Y-%m-%d %H:%M:%S"))

# Inicia a geração de dados em segundo plano
threading.Thread(target=gerar_dados_sensores, daemon=True).start()

# Executa o servidor web
if __name__ == '__main__':
    print("🚀 Servidor de simulação IoT iniciado!")
    print("📊 Acesse: http://localhost:5000")
    print("🔄 Dados atualizando a cada 3 segundos...")
    app.run(debug=True, use_reloader=False)
------------------------------------------------------------------------------------------------------------------------|
2° Versão: 
📍 Versão 2.0 - Sistema Web Básico
Data: Primeira implementação web
Funcionalidades:

✅ Servidor Flask local

✅ 4 sensores simulados (temperatura, água, movimento, acesso)

✅ Interface web automática

✅ Atualização a cada 3 segundos

✅ Design visual básico
------------------------------------------------------------------------------------------------------------------------|
Código:
# app.py - Sistema Completo de Monitoramento de Horta Inteligente
from flask import Flask, render_template_string
import random
import threading
import time
from datetime import datetime
import json

app = Flask(__name__)

# Dados dos sensores
dados_sensores = {
    "meteorologia": {
        "temperatura": 25.0,
        "umidade_ar": 60,
        "possibilidade_chuva": 30,
        "sensacao_termica": 26.0,
        "precipitacao_termica": 0
    },
    "agua": {
        "nivel": 50,
        "bomba_ligada": False,
        "historico_bomba": []
    },
    "seguranca": {
        "movimento_detectado": False,
        "pessoa_detectada": "Ninguém",
        "autorizado": False,
        "alarme_ativado": False,
        "historico_acessos": []
    }
}

# Banco de dados de pessoas autorizadas
pessoas_autorizadas = {
    "joao": "João Silva - Proprietário",
    "maria": "Maria Santos - Jardineira",
    "carlos": "Carlos Oliveira - Técnico"
}

# Gera dados meteorológicos realistas
def gerar_dados_meteorologicos():
    while True:
        temp = round(random.uniform(18.0, 32.0), 1)
        umidade = random.randint(40, 90)
        chance_chuva = random.randint(0, 100)
        
        # Cálculo da sensação térmica (simplificado)
        sensacao = temp
        if umidade > 70:
            sensacao += round((umidade - 70) * 0.1, 1)
        
        # Precipitação térmica (simulação)
        precipitacao = 0
        if chance_chuva > 70 and umidade > 75:
            precipitacao = random.randint(1, 10)
        
        dados_sensores['meteorologia'] = {
            "temperatura": temp,
            "umidade_ar": umidade,
            "possibilidade_chuva": chance_chuva,
            "sensacao_termica": sensacao,
            "precipitacao_termica": precipitacao
        }
        time.sleep(5)

# Simula o sistema de água e irrigação
def simular_sistema_agua():
    while True:
        nivel = dados_sensores['agua']['nivel']
        bomba_ligada = False
        
        # Lógica de acionamento da bomba
        if nivel < 30 and not dados_sensores['agua']['bomba_ligada']:
            bomba_ligada = True
            evento = {
                "horario": datetime.now().strftime("%H:%M:%S"),
                "acao": "Bomba LIGADA",
                "nivel_antes": nivel,
                "nivel_depois": nivel + 40
            }
            dados_sensores['agua']['historico_bomba'].append(evento)
            dados_sensores['agua']['nivel'] = nivel + 40
        
        elif dados_sensores['agua']['bomba_ligada'] and nivel > 80:
            bomba_ligada = False
            evento = {
                "horario": datetime.now().strftime("%H:%M:%S"),
                "acao": "Bomba DESLIGADA",
                "nivel_antes": nivel,
                "nivel_depois": nivel
            }
            dados_sensores['agua']['historico_bomba'].append(evento)
        
        # Consumo gradual de água
        if not bomba_ligada:
            dados_sensores['agua']['nivel'] = max(0, nivel - random.randint(1, 3))
        
        dados_sensores['agua']['bomba_ligada'] = bomba_ligada
        time.sleep(3)

# Simula detecção de movimento e reconhecimento
def simular_sistema_seguranca():
    while True:
        # Simula detecção a cada 10 segundos
        time.sleep(10)
        
        if random.random() > 0.7:  # 30% de chance de detecção
            pessoa_id = random.choice(list(pessoas_autorizadas.keys()) + ["intruso"])
            
            if pessoa_id == "intruso":
                dados_sensores['seguranca'] = {
                    "movimento_detectado": True,
                    "pessoa_detectada": "INTRUSO NÃO AUTORIZADO!",
                    "autorizado": False,
                    "alarme_ativado": True,
                    "historico_acessos": dados_sensores['seguranca']['historico_acessos']
                }
                # Mantém o alarme por 5 segundos
                time.sleep(5)
                dados_sensores['seguranca']['alarme_ativado'] = False
            else:
                dados_sensores['seguranca'] = {
                    "movimento_detectado": True,
                    "pessoa_detectada": pessoas_autorizadas[pessoa_id],
                    "autorizado": True,
                    "alarme_ativado": False,
                    "historico_acessos": dados_sensores['seguranca']['historico_acessos']
                }
                
                # Registra o acesso
                acesso = {
                    "horario": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
                    "pessoa": pessoas_autorizadas[pessoa_id],
                    "autorizado": True
                }
                dados_sensores['seguranca']['historico_acessos'].append(acesso)
                
                # Limita o histórico aos últimos 10 eventos
                if len(dados_sensores['seguranca']['historico_acessos']) > 10:
                    dados_sensores['seguranca']['historico_acessos'].pop(0)
        else:
            dados_sensores['seguranca']['movimento_detectado'] = False

# Página HTML completa
HTML_PAGE = """
<!DOCTYPE html>
<html>
<head>
    <title>Sistema Inteligente de Horta</title>
    <meta http-equiv="refresh" content="5">
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            margin: 0; 
            padding: 20px; 
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        
        h1 { 
            text-align: center; 
            color: #2c3e50; 
            margin-bottom: 30px;
        }
        
        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .card {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            border-left: 4px solid #3498db;
        }
        
        .card.alarme {
            border-left-color: #e74c3c;
            background: #ffe6e6;
            animation: piscar 1s infinite;
        }
        
        .card.autorizado {
            border-left-color: #27ae60;
            background: #e6ffe6;
        }
        
        .card.bomba {
            border-left-color: #f39c12;
            background: #fff3e6;
        }
        
        @keyframes piscar {
            0% { opacity: 1; }
            50% { opacity: 0.7; }
            100% { opacity: 1; }
        }
        
        .valor {
            font-size: 28px;
            font-weight: bold;
            color: #2c3e50;
            margin: 10px 0;
        }
        
        .info {
            color: #7f8c8d;
            font-size: 14px;
        }
        
        .historico {
            background: white;
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
        }
        
        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: #f2f2f2;
        }
        
        .status {
            padding: 5px 10px;
            border-radius: 5px;
            font-weight: bold;
        }
        
        .autorizado { background: #d4edda; color: #155724; }
        .nao-autorizado { background: #f8d7da; color: #721c24; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🌿 Sistema Inteligente de Monitoramento de Horta</h1>
        
        <div class="dashboard">
            <!-- Meteorologia -->
            <div class="card">
                <h2>🌤️ Meteorologia</h2>
                <div class="valor">{{ meteo.temperatura }}°C</div>
                <div class="info">Sensação: {{ meteo.sensacao_termica }}°C</div>
                <div class="info">Umidade: {{ meteo.umidade_ar }}%</div>
                <div class="info">Chuva: {{ meteo.possibilidade_chuva }}%</div>
                <div class="info">Precipitação: {{ meteo.precipitacao_termica }}mm</div>
            </div>
            
            <!-- Sistema de Água -->
            <div class="card {% if agua.bomba_ligada %}bomba{% endif %}">
                <h2>💧 Sistema de Água</h2>
                <div class="valor">{{ agua.nivel }}%</div>
                <div class="info">Bomba: {% if agua.bomba_ligada %}🔴 LIGADA{% else %}🟢 DESLIGADA{% endif %}</div>
                <div class="info">Última irrigação: {% if agua.historico_bomba %}{{ agua.historico_bomba[-1].horario }}{% else %}Nenhuma{% endif %}</div>
            </div>
            
            <!-- Segurança -->
            <div class="card {% if seguranca.alarme_ativado %}alarme{% elif seguranca.autorizado %}autorizado{% endif %}">
                <h2>🔐 Sistema de Segurança</h2>
                <div class="valor">{% if seguranca.movimento_detectado %}{{ seguranca.pessoa_detectada }}{% else %}Sem movimento{% endif %}</div>
                <div class="info">Status: {% if seguranca.alarme_ativado %}🚨 ALARME ATIVADO{% elif seguranca.autorizado %}✅ Autorizado{% else %}⏳ Monitorando{% endif %}</div>
            </div>
        </div>
        
        <!-- Histórico -->
        <div class="historico">
            <h2>📊 Histórico de Eventos</h2>
            <table>
                <tr>
                    <th>Horário</th>
                    <th>Evento</th>
                    <th>Detalhes</th>
                </tr>
                {% for evento in agua.historico_bomba[-5:]|reverse %}
                <tr>
                    <td>{{ evento.horario }}</td>
                    <td>🚰 Irrigação</td>
                    <td>{{ evento.acao }} - Nível: {{ evento.nivel_antes }}% → {{ evento.nivel_depois }}%</td>
                </tr>
                {% endfor %}
                {% for acesso in seguranca.historico_acessos[-5:]|reverse %}
                <tr>
                    <td>{{ acesso.horario }}</td>
                    <td>👤 Acesso</td>
                    <td><span class="status {% if acesso.autorizado %}autorizado{% else %}nao-autorizado{% endif %}">
                        {{ acesso.pessoa }}
                    </span></td>
                </tr>
                {% endfor %}
            </table>
        </div>
        
        <div class="info" style="text-align: center; margin-top: 30px;">
            Última atualização: {{ timestamp }}
        </div>
    </div>
</body>
</html>
"""

@app.route('/')
def index():
    return render_template_string(HTML_PAGE, 
                                meteo=dados_sensores['meteorologia'],
                                agua=dados_sensores['agua'],
                                seguranca=dados_sensores['seguranca'],
                                timestamp=datetime.now().strftime("%Y-%m-%d %H:%M:%S"))

# Inicia todas as simulações
threading.Thread(target=gerar_dados_meteorologicos, daemon=True).start()
threading.Thread(target=simular_sistema_agua, daemon=True).start()
threading.Thread(target=simular_sistema_seguranca, daemon=True).start()

if __name__ == '__main__':
    print("🌿 Sistema Inteligente de Horta Iniciado!")
    print("📊 Acesse: http://localhost:5000")
    print("🔄 Dados atualizando automaticamente...")
    app.run(debug=True, use_reloader=False)
------------------------------------------------------------------------------------------------------------------------|
3° Versão: 
Data: Funcionalidades finais
Funcionalidades:

✅ Geração de PDF automática

✅ Relatório completo com todos os dados

✅ Botão de download na interface

✅ Formatação profissional do PDF

✅ Histórico de eventos exportável
------------------------------------------------------------------------------------------------------------------------|
Código:
# app.py - Sistema Corrigido com UTF-8 para PDF
from flask import Flask, render_template_string, make_response
import random
import threading
import time
from datetime import datetime, timedelta
from fpdf import FPDF
import io

app = Flask(__name__)

# Dados dos sensores
dados_sensores = {
    "meteorologia": {
        "temperatura": 25.0,
        "umidade_ar": 60,
        "possibilidade_chuva": 30,
        "sensacao_termica": 26.0,
        "precipitacao_termica": 0,
        "radiacao_solar": 0,
        "condicao_plantio": "Analisando..."
    },
    "agua": {
        "nivel": 50,
        "bomba_ligada": False,
        "historico_bomba": [],
        "tempo_total_ligada": "00:00:00",
        "ultimo_acionamento": None
    },
    "seguranca": {
        "movimento_detectado": False,
        "pessoa_detectada": "Ninguém",
        "autorizado": False,
        "alarme_ativado": False,
        "historico_acessos": [],
        "pessoa_no_local": False,
        "entrada_time": None,
        "tempo_permanencia": "00:00:00"
    }
}

# Banco de dados de pessoas autorizadas
pessoas_autorizadas = {
    "joao": {"nome": "João Silva - Proprietário", "tempo_medio": timedelta(minutes=15)},
    "maria": {"nome": "Maria Santos - Jardineira", "tempo_medio": timedelta(minutes=45)},
    "carlos": {"nome": "Carlos Oliveira - Técnico", "tempo_medio": timedelta(minutes=30)}
}

# Gera dados meteorológicos realistas
def gerar_dados_meteorologicos():
    while True:
        temp = round(random.uniform(18.0, 32.0), 1)
        umidade = random.randint(40, 90)
        chance_chuva = random.randint(0, 100)
        radiacao = random.randint(200, 1200)
        
        sensacao = temp
        if umidade > 70:
            sensacao += round((umidade - 70) * 0.1, 1)
        
        precipitacao = 0
        if chance_chuva > 70 and umidade > 75:
            precipitacao = random.randint(1, 10)
        
        condicao = analisar_condicao_plantio(temp, umidade, radiacao, chance_chuva)
        
        dados_sensores['meteorologia'] = {
            "temperatura": temp,
            "umidade_ar": umidade,
            "possibilidade_chuva": chance_chuva,
            "sensacao_termica": sensacao,
            "precipitacao_termica": precipitacao,
            "radiacao_solar": radiacao,
            "condicao_plantio": condicao
        }
        time.sleep(5)

def analisar_condicao_plantio(temp, umidade, radiacao, chuva):
    pontuacao = 0
    
    if 20 <= temp <= 28:
        pontuacao += 2
    elif 15 <= temp <= 30:
        pontuacao += 1
    
    if 50 <= umidade <= 80:
        pontuacao += 2
    elif 40 <= umidade <= 90:
        pontuacao += 1
    
    if 400 <= radiacao <= 900:
        pontuacao += 2
    elif 300 <= radiacao <= 1000:
        pontuacao += 1
    
    if chuva < 30:
        pontuacao += 2
    elif chuva < 50:
        pontuacao += 1
    
    if pontuacao >= 6:
        return "Excelente para plantar!"
    elif pontuacao >= 4:
        return "Bom para plantar ou colher"
    elif pontuacao >= 2:
        return "Condições regulares"
    else:
        return "Não recomendado"

# Simula o sistema de água e irrigação
def simular_sistema_agua():
    while True:
        nivel = dados_sensores['agua']['nivel']
        bomba_ligada = dados_sensores['agua']['bomba_ligada']
        
        if bomba_ligada and dados_sensores['agua']['ultimo_acionamento']:
            tempo_decorrido = datetime.now() - dados_sensores['agua']['ultimo_acionamento']
            total_segundos = int(dados_sensores['agua']['tempo_total_ligada'].split(':')[0]) * 3600 + \
                            int(dados_sensores['agua']['tempo_total_ligada'].split(':')[1]) * 60 + \
                            int(dados_sensores['agua']['tempo_total_ligada'].split(':')[2])
            total_segundos += int(tempo_decorrido.total_seconds())
            horas = total_segundos // 3600
            minutos = (total_segundos % 3600) // 60
            segundos = total_segundos % 60
            dados_sensores['agua']['tempo_total_ligada'] = f"{horas:02d}:{minutos:02d}:{segundos:02d}"
            dados_sensores['agua']['ultimo_acionamento'] = datetime.now()
        
        if nivel < 30 and not bomba_ligada:
            dados_sensores['agua']['bomba_ligada'] = True
            dados_sensores['agua']['ultimo_acionamento'] = datetime.now()
            
            evento = {
                "horario": datetime.now().strftime("%H:%M:%S"),
                "acao": "Bomba LIGADA",
                "nivel_antes": nivel,
                "nivel_depois": nivel + 40,
                "duracao": "00:00:00",
                "inicio": datetime.now()
            }
            dados_sensores['agua']['historico_bomba'].append(evento)
            dados_sensores['agua']['nivel'] = nivel + 40
        
        elif bomba_ligada and nivel > 80:
            dados_sensores['agua']['bomba_ligada'] = False
            
            if dados_sensores['agua']['historico_bomba']:
                ultimo_evento = dados_sensores['agua']['historico_bomba'][-1]
                if ultimo_evento['acao'] == "Bomba LIGADA":
                    duracao = datetime.now() - ultimo_evento['inicio']
                    horas = duracao.seconds // 3600
                    minutos = (duracao.seconds % 3600) // 60
                    segundos = duracao.seconds % 60
                    ultimo_evento['duracao'] = f"{horas:02d}:{minutos:02d}:{segundos:02d}"
            
            evento = {
                "horario": datetime.now().strftime("%H:%M:%S"),
                "acao": "Bomba DESLIGADA",
                "nivel_antes": nivel,
                "nivel_depois": nivel,
                "duracao": "",
                "inicio": None
            }
            dados_sensores['agua']['historico_bomba'].append(evento)
        
        if not dados_sensores['agua']['bomba_ligada']:
            dados_sensores['agua']['nivel'] = max(0, nivel - random.randint(1, 3))
        
        time.sleep(3)

# Simula detecção de movimento e reconhecimento
def simular_sistema_seguranca():
    while True:
        time.sleep(5)
        
        if dados_sensores['seguranca']['pessoa_no_local'] and dados_sensores['seguranca']['entrada_time']:
            tempo_permanencia = datetime.now() - dados_sensores['seguranca']['entrada_time']
            horas = tempo_permanencia.seconds // 3600
            minutos = (tempo_permanencia.seconds % 3600) // 60
            segundos = tempo_permanencia.seconds % 60
            dados_sensores['seguranca']['tempo_permanencia'] = f"{horas:02d}:{minutos:02d}:{segundos:02d}"
        
        if random.random() > 0.8:
            pessoa_id = random.choice(list(pessoas_autorizadas.keys()) + ["intruso"])
            
            if pessoa_id == "intruso":
                dados_sensores['seguranca'].update({
                    "movimento_detectado": True,
                    "pessoa_detectada": "INTRUSO NÃO AUTORIZADO!",
                    "autorizado": False,
                    "alarme_ativado": True,
                    "pessoa_no_local": True,
                    "entrada_time": datetime.now()
                })
                time.sleep(3)
                dados_sensores['seguranca']['alarme_ativado'] = False
                dados_sensores['seguranca']['pessoa_no_local'] = False
                dados_sensores['seguranca']['entrada_time'] = None
                
            else:
                dados_sensores['seguranca'].update({
                    "movimento_detectado": True,
                    "pessoa_detectada": pessoas_autorizadas[pessoa_id]['nome'],
                    "autorizado": True,
                    "alarme_ativado": False,
                    "pessoa_no_local": True,
                    "entrada_time": datetime.now()
                })
                
                acesso = {
                    "horario": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
                    "pessoa": pessoas_autorizadas[pessoa_id]['nome'],
                    "autorizado": True,
                    "tempo_permanencia": "00:00:00",
                    "entrada": datetime.now()
                }
                dados_sensores['seguranca']['historico_acessos'].append(acesso)
                
                tempo_medio = pessoas_autorizadas[pessoa_id]['tempo_medio'].total_seconds()
                time.sleep(min(5, tempo_medio))
                
                if dados_sensores['seguranca']['historico_acessos']:
                    ultimo_acesso = dados_sensores['seguranca']['historico_acessos'][-1]
                    tempo_permanencia = datetime.now() - ultimo_acesso['entrada']
                    horas = tempo_permanencia.seconds // 3600
                    minutos = (tempo_permanencia.seconds % 3600) // 60
                    segundos = tempo_permanencia.seconds % 60
                    ultimo_acesso['tempo_permanencia'] = f"{horas:02d}:{minutos:02d}:{segundos:02d}"
                
                dados_sensores['seguranca']['pessoa_no_local'] = False
                dados_sensores['seguranca']['entrada_time'] = None
                dados_sensores['seguranca']['movimento_detectado'] = False
                
        else:
            dados_sensores['seguranca']['movimento_detectado'] = False

# Geração de PDF CORRIGIDA - SEM EMOJIS
def gerar_pdf():
    try:
        pdf = FPDF()
        pdf.add_page()
        
        # Título
        pdf.set_font("Arial", 'B', 16)
        pdf.cell(0, 10, "Relatorio da Horta Inteligente", 0, 1, 'C')
        pdf.ln(5)
        
        # Data e hora
        pdf.set_font("Arial", '', 12)
        pdf.cell(0, 8, f"Data: {datetime.now().strftime('%d/%m/%Y %H:%M:%S')}", 0, 1)
        pdf.ln(5)
        
        # Meteorologia
        pdf.set_font("Arial", 'B', 14)
        pdf.cell(0, 10, "Dados Meteorologicos", 0, 1)
        pdf.set_font("Arial", '', 12)
        meteo = dados_sensores['meteorologia']
        pdf.cell(0, 8, f"Temperatura: {meteo['temperatura']} C", 0, 1)
        pdf.cell(0, 8, f"Sensacao Termica: {meteo['sensacao_termica']} C", 0, 1)
        pdf.cell(0, 8, f"Umidade: {meteo['umidade_ar']}%", 0, 1)
        pdf.cell(0, 8, f"Radiacao Solar: {meteo['radiacao_solar']} W/m2", 0, 1)
        pdf.cell(0, 8, f"Chuva: {meteo['possibilidade_chuva']}%", 0, 1)
        pdf.cell(0, 8, f"Condicao: {meteo['condicao_plantio']}", 0, 1)
        pdf.ln(5)
        
        # Água
        pdf.set_font("Arial", 'B', 14)
        pdf.cell(0, 10, "Sistema de Agua", 0, 1)
        pdf.set_font("Arial", '', 12)
        agua = dados_sensores['agua']
        pdf.cell(0, 8, f"Nivel: {agua['nivel']}%", 0, 1)
        pdf.cell(0, 8, f"Bomba: {'LIGADA' if agua['bomba_ligada'] else 'DESLIGADA'}", 0, 1)
        pdf.cell(0, 8, f"Tempo Total Ligada: {agua['tempo_total_ligada']}", 0, 1)
        pdf.ln(5)
        
        # Segurança
        pdf.set_font("Arial", 'B', 14)
        pdf.cell(0, 10, "Sistema de Seguranca", 0, 1)
        pdf.set_font("Arial", '', 12)
        seg = dados_sensores['seguranca']
        if seg['pessoa_no_local']:
            pdf.cell(0, 8, f"Pessoa no local: {seg['pessoa_detectada']}", 0, 1)
            pdf.cell(0, 8, f"Tempo de permanencia: {seg['tempo_permanencia']}", 0, 1)
        pdf.ln(5)
        
        # Histórico
        pdf.set_font("Arial", 'B', 14)
        pdf.cell(0, 10, "Historico de Eventos", 0, 1)
        pdf.set_font("Arial", '', 10)
        
        # Histórico da bomba
        for evento in agua['historico_bomba'][-5:]:
            linha = f"{evento['horario']} - {evento['acao']}"
            if evento['duracao'] and evento['duracao'] != "00:00:00":
                linha += f" ({evento['duracao']})"
            pdf.cell(0, 6, linha, 0, 1)
        
        # Histórico de acessos
        for acesso in seg['historico_acessos'][-3:]:
            linha = f"{acesso['horario']} - {acesso['pessoa']}"
            if acesso['tempo_permanencia']:
                linha += f" ({acesso['tempo_permanencia']})"
            pdf.cell(0, 6, linha, 0, 1)
        
        # Gera o PDF em memória
        return pdf.output(dest='S').encode('latin1', 'replace')
        
    except Exception as e:
        print(f"Erro ao gerar PDF: {e}")
        pdf = FPDF()
        pdf.add_page()
        pdf.set_font("Arial", 'B', 16)
        pdf.cell(0, 10, "Erro ao gerar relatorio", 0, 1, 'C')
        pdf.set_font("Arial", '', 12)
        pdf.cell(0, 10, f"Erro: {str(e)}", 0, 1)
        return pdf.output(dest='S').encode('latin1', 'replace')

@app.route('/')
def index():
    return render_template_string(HTML_PAGE, 
                                meteo=dados_sensores['meteorologia'],
                                agua=dados_sensores['agua'],
                                seguranca=dados_sensores['seguranca'],
                                timestamp=datetime.now().strftime("%Y-%m-%d %H:%M:%S"))

@app.route('/gerar-pdf')
def download_pdf():
    try:
        pdf_data = gerar_pdf()
        response = make_response(pdf_data)
        response.headers['Content-Type'] = 'application/pdf'
        response.headers['Content-Disposition'] = 'attachment; filename=relatorio_horta.pdf'
        return response
    except Exception as e:
        return f"Erro ao gerar PDF: {str(e)}", 500

# HTML completo (mantendo emojis apenas na web)
HTML_PAGE = """
<!DOCTYPE html>
<html>
<head>
    <title>Sistema Inteligente de Horta</title>
    <meta http-equiv="refresh" content="5">
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            margin: 0; 
            padding: 20px; 
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            min-height: 100vh;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        
        h1 { 
            text-align: center; 
            color: #2c3e50; 
            margin-bottom: 30px;
        }
        
        .header-buttons {
            text-align: center;
            margin-bottom: 20px;
        }
        
        .btn-pdf {
            background: #e74c3c;
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            text-decoration: none;
            display: inline-block;
        }
        
        .btn-pdf:hover {
            background: #c0392b;
        }
        
        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .card {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            border-left: 4px solid #3498db;
        }
        
        .card.alarme {
            border-left-color: #e74c3c;
            background: #ffe6e6;
            animation: piscar 1s infinite;
        }
        
        .card.autorizado {
            border-left-color: #27ae60;
            background: #e6ffe6;
        }
        
        .card.bomba {
            border-left-color: #f39c12;
            background: #fff3e6;
        }
        
        .card.plantio {
            border-left-color: #9b59b6;
            background: #f8f0ff;
        }
        
        @keyframes piscar {
            0% { opacity: 1; }
            50% { opacity: 0.7; }
            100% { opacity: 1; }
        }
        
        .valor {
            font-size: 28px;
            font-weight: bold;
            color: #2c3e50;
            margin: 10px 0;
        }
        
        .info {
            color: #7f8c8d;
            font-size: 14px;
            margin: 5px 0;
        }
        
        .info-tempo {
            color: #2980b9;
            font-weight: bold;
        }
        
        .historico {
            background: white;
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
        }
        
        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: #f2f2f2;
        }
        
        .status {
            padding: 5px 10px;
            border-radius: 5px;
            font-weight: bold;
        }
        
        .autorizado { background: #d4edda; color: #155724; }
        .nao-autorizado { background: #f8d7da; color: #721c24; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🌿 Sistema Inteligente de Monitoramento de Horta</h1>
        
        <div class="header-buttons">
            <a href="/gerar-pdf" class="btn-pdf">📄 Gerar PDF do Relatório</a>
        </div>
        
        <div class="dashboard">
            <!-- Meteorologia -->
            <div class="card">
                <h2>🌤️ Meteorologia</h2>
                <div class="valor">{{ meteo.temperatura }}°C</div>
                <div class="info">Sensação: {{ meteo.sensacao_termica }}°C</div>
                <div class="info">Umidade: {{ meteo.umidade_ar }}%</div>
                <div class="info">Radiação Solar: {{ meteo.radiacao_solar }} W/m²</div>
                <div class="info">Chuva: {{ meteo.possibilidade_chuva }}%</div>
            </div>
            
            <!-- Condição de Plantio -->
            <div class="card plantio">
                <h2>🌱 Condição para Plantio</h2>
                <div class="valor">{{ meteo.condicao_plantio }}</div>
            </div>
            
            <!-- Sistema de Água -->
            <div class="card {% if agua.bomba_ligada %}bomba{% endif %}">
                <h2>💧 Sistema de Água</h2>
                <div class="valor">{{ agua.nivel }}%</div>
                <div class="info">Bomba: {% if agua.bomba_ligada %}🔴 LIGADA{% else %}🟢 DESLIGADA{% endif %}</div>
                <div class="info-tempo">Tempo Total Ligada: {{ agua.tempo_total_ligada }}</div>
                {% if agua.historico_bomba and agua.historico_bomba[-1].duracao %}
                <div class="info">Última duração: {{ agua.historico_bomba[-1].duracao }}</div>
                {% endif %}
            </div>
            
            <!-- Segurança -->
            <div class="card {% if seguranca.alarme_ativado %}alarme{% elif seguranca.autorizado %}autorizado{% endif %}">
                <h2>🔐 Sistema de Segurança</h2>
                <div class="valor">{% if seguranca.movimento_detectado %}{{ seguranca.pessoa_detectada }}{% else %}Sem movimento{% endif %}</div>
                {% if seguranca.pessoa_no_local %}
                <div class="info-tempo">Tempo no local: {{ seguranca.tempo_permanencia }}</div>
                {% endif %}
                <div class="info">Status: {% if seguranca.alarme_ativado %}🚨 ALARME ATIVADO{% elif seguranca.autorizado %}✅ Autorizado{% else %}⏳ Monitorando{% endif %}</div>
            </div>
        </div>
        
        <!-- Histórico -->
        <div class="historico">
            <h2>📊 Histórico de Eventos</h2>
            <table>
                <tr>
                    <th>Horário</th>
                    <th>Evento</th>
                    <th>Detalhes</th>
                    <th>Duração</th>
                </tr>
                {% for evento in agua.historico_bomba[-5:]|reverse %}
                <tr>
                    <td>{{ evento.horario }}</td>
                    <td>🚰 {{ evento.acao }}</td>
                    <td>Nível: {{ evento.nivel_antes }}% → {{ evento.nivel_depois }}%</td>
                    <td class="info-tempo">{% if evento.duracao and evento.duracao != "00:00:00" %}{{ evento.duracao }}{% endif %}</td>
                </tr>
                {% endfor %}
                {% for acesso in seguranca.historico_acessos[-5:]|reverse %}
                <tr>
                    <td>{{ acesso.horario }}</td>
                    <td>👤 Acesso</td>
                    <td><span class="status {% if acesso.autorizado %}autorizado{% else %}nao-autorizado{% endif %}">
                        {{ acesso.pessoa }}
                    </span></td>
                    <td class="info-tempo">{% if acesso.tempo_permanencia %}{{ acesso.tempo_permanencia }}{% endif %}</td>
                </tr>
                {% endfor %}
            </table>
        </div>
        
        <div class="info" style="text-align: center; margin-top: 30px;">
            Última atualização: {{ timestamp }}
        </div>
    </div>
</body>
</html>
"""

# Inicia todas as simulações
threading.Thread(target=gerar_dados_meteorologicos, daemon=True).start()
threading.Thread(target=simular_sistema_agua, daemon=True).start()
threading.Thread(target=simular_sistema_seguranca, daemon=True).start()

if __name__ == '__main__':
    print("🌿 Sistema Inteligente de Horta Iniciado!")
    print("📊 Acesse: http://localhost:5000")
    print("📄 PDF disponível em: http://localhost:5000/gerar-pdf")
    print("🔄 Dados atualizando automaticamente...")
    
    try:
        from fpdf import FPDF
        print("✅ Biblioteca PDF pronta!")
    except ImportError:
        print("⚠️  Instalando biblioteca FPDF...")
        import subprocess
        subprocess.run(["pip", "install", "fpdf"])
        print("✅ Biblioteca FPDF instalada!")
    
    app.run(debug=True, use_reloader=False)

