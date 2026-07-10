👥 Integrantes
Nome completo	GitHub	Turma
(janaína)	@nome de usuário	3A
(flávia)	@nome de usuário	3A
(sara)	@nome de usuário	3A
Tema: (Sistema de conscientização e apoio no combate ao tráfico de mulheres e adolescentes.) 
Tecnologia: Python + Flask + SQLite

# O que o sistema faz:
O sistema tem como finalidade informar e conscientizar a sociedade sobre o tráfico de mulheres e adolescentes, disponibilizando conteúdos relacionados à prevenção, ao reconhecimento de situações de risco e aos canais oficiais de denúncia e assistência às vítimas. A plataforma também visa ampliar o acesso a informações seguras e confiáveis, promovendo a prevenção desse tipo de crime e incentivando a denúncia de casos suspeitos.

# Como o grupo trabalha toda semana:
Segunda-feira: cada membro abre as Edições da semana.
Durante a semana: desenvolvimento das tarefas e realização dos commits.
Sexta-feira: o grupo abre um Pull Request relacionando os Issues concluídos.
Push: as avaliações de participação são atualizadas automaticamente pelo GitHub Actions.
# estrutura do projeto:
# Guardar este ficheiro como src/app.py
from flask import Flask, render_template, request, redirect, url_path
import sqlite3

app = Flask(__name__)


# Configuração inicial da base de dados SQLite
def init_db():
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    # Tabela para registar denúncias ou relatos anónimos sobre rotas/vulnerabilidades
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS denuncias (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            localizacao TEXT NOT None,
            descricao TEXT NOT None,
            data_registo TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
    ''')
    conn.commit()
    conn.close()

@app.route('/')
def index():
    return "<h1>Plataforma de Combate ao Tráfico de Mulheres - Grupo 05</h1><p>Sistema em desenvolvimento.</p>"

@app.route('/denunciar', methods=['POST'])
def criar_denuncia():
    localizacao = request.form.get('localizacao')
    descricao = request.form.get('descricao')
    
    if localizacao and descricao:
        conn = sqlite3.connect('database.db')
        cursor = conn.cursor()
        cursor.execute('INSERT INTO denuncias (localizacao, descricao) VALUES (?, ?)', (localizacao, descricao))
        conn.commit()
        conn.close()
    return redirect('/')

if __name__ == '__main__':
    init_db()
    app.run(debug=True)
