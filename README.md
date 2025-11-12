import sqlite3
import os

# =====================================
# ETAPA 1 — CRIAR BANCO DE DADOS
# =====================================

# Verifica se o banco já existe (BÔNUS)
db_name = "escola.db"
existe = os.path.exists(db_name)

# Conexão com o banco
conn = sqlite3.connect(db_name)
cur = conn.cursor()

# Cria tabela somente se ainda não existir
if not existe:
    cur.execute("""
        CREATE TABLE alunos (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            nome TEXT NOT NULL,
            idade INTEGER,
            email TEXT
        )
    """)
    print("✅ Banco e tabela criados com sucesso!")
else:
    print("⚙️ Banco já existente, conectando...")

# =====================================
# ETAPA 2 — INSERIR REGISTROS
# =====================================

def inserir_aluno(nome, idade, email):
    cur.execute("INSERT INTO alunos (nome, idade, email) VALUES (?, ?, ?)", (nome, idade, email))
    conn.commit()
    print(f"✅ Aluno '{nome}' inserido com sucesso!")

# Lista com 40 alunos
alunos = [
    ("Rebeca Vieira", 15, "rebeca.vieira@email.com"),
    ("Eshyley Castro", 16, "eshyley.castro@email.com"),
    ("Ana Castela", 17, "ana.castela@email.com"),
    ("João Pedro", 16, "joao.pedro@email.com"),
    ("Mariana Silva", 15, "mariana.silva@email.com"),
    ("Lucas Oliveira", 18, "lucas.oliveira@email.com"),
    ("Bianca Souza", 17, "bianca.souza@email.com"),
    ("Pedro Henrique", 16, "pedro.henrique@email.com"),
    ("Juliana Andrade", 15, "juliana.andrade@email.com"),
    ("Gustavo Lima", 17, "gustavo.lima@email.com"),
    ("Camila Rocha", 16, "camila.rocha@email.com"),
    ("Felipe Santos", 18, "felipe.santos@email.com"),
    ("Larissa Gomes", 15, "larissa.gomes@email.com"),
    ("Bruno Martins", 17, "bruno.martins@email.com"),
    ("Vitória Almeida", 16, "vitoria.almeida@email.com"),
    ("Mateus Nogueira", 17, "mateus.nogueira@email.com"),
    ("Isabela Torres", 18, "isabela.torres@email.com"),
    ("Eduardo Pires", 15, "eduardo.pires@email.com"),
    ("Fernanda Costa", 16, "fernanda.costa@email.com"),
    ("Ricardo Ramos", 17, "ricardo.ramos@email.com"),
    ("Carolina Teixeira", 18, "carolina.teixeira@email.com"),
    ("Daniel Moreira", 15, "daniel.moreira@email.com"),
    ("Nicole Ferreira", 17, "nicole.ferreira@email.com"),
    ("Vinícius Melo", 16, "vinicius.melo@email.com"),
    ("Sofia Barbosa", 15, "sofia.barbosa@email.com"),
    ("Caio Carvalho", 18, "caio.carvalho@email.com"),
    ("Beatriz Azevedo", 17, "beatriz.azevedo@email.com"),
    ("André Lima", 16, "andre.lima@email.com"),
    ("Letícia Moura", 15, "leticia.moura@email.com"),
    ("Thiago Correia", 17, "thiago.correia@email.com"),
    ("Giovana Duarte", 16, "giovana.duarte@email.com"),
    ("Henrique Ribeiro", 18, "henrique.ribeiro@email.com"),
    ("Laura Pacheco", 15, "laura.pacheco@email.com"),
    ("Rafael Tavares", 17, "rafael.tavares@email.com"),
    ("Manuela Freitas", 16, "manuela.freitas@email.com"),
    ("Diego Nunes", 18, "diego.nunes@email.com"),
    ("Clara Fernandes", 15, "clara.fernandes@email.com"),
    ("Murilo Cardoso", 17, "murilo.cardoso@email.com"),
    ("Helena Faria", 16, "helena.faria@email.com"),
    ("Leonardo Almeida", 18, "leonardo.almeida@email.com")
    
]


# Inserir os 40 alunos
for nome, idade, email in alunos:
    inserir_aluno(nome, idade, email)

# =====================================
# ETAPA 3 — LISTAR TODOS
# =====================================

def listar_alunos():
    print("\n📋 Lista de alunos:")
    for aluno in cur.execute("SELECT * FROM alunos"):
        print(aluno)

listar_alunos()

# =====================================
# ETAPA 4 — BUSCAR POR ID
# =====================================

def buscar_por_id(id_aluno):
    cur.execute("SELECT * FROM alunos WHERE id = ?", (id_aluno,))
    resultado = cur.fetchone()
    if resultado:
        print(f"\n🔎 Resultado da busca (ID {id_aluno}): {resultado}")
    else:
        print(f"\n⚠️ Nenhum aluno encontrado com o ID {id_aluno}.")

buscar_por_id(5)

# =====================================
# ETAPA 5 — ATUALIZAR REGISTROS
# =====================================

def atualizar_aluno(id_aluno, novo_nome, nova_idade, novo_email):
    cur.execute("""
        UPDATE alunos
        SET nome = ?, idade = ?, email = ?
        WHERE id = ?
    """, (novo_nome, nova_idade, novo_email, id_aluno))
    conn.commit()
    print(f"\n✏️ Aluno com ID {id_aluno} atualizado com sucesso!")
    buscar_por_id(id_aluno)

# Exemplo: atualizar aluno ID 1
atualizar_aluno(1, "Rebeca Vieira Castela", 15, "rebeca.castela@email.com")

# =====================================
# ETAPA 6 — EXCLUIR REGISTROS
# =====================================

def excluir_aluno(id_aluno):
    cur.execute("DELETE FROM alunos WHERE id = ?", (id_aluno,))
    conn.commit()
    print(f"\n🗑️ Aluno com ID {id_aluno} excluído com sucesso!")
    listar_alunos()

# Exemplo: excluir aluno ID 40
excluir_aluno(40)

# =====================================
# FINALIZAÇÃO
# =====================================
conn.close()
print("\n✅ Conexão encerrada com sucesso.")
