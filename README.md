# Projeto1

## Modelo Entidade Relacionamento:

![alt text](image-1.png)


## Modelo relacional na 3FN (Normalização):


## Queries para a criação das tabelas necessárias:

```
    CREATE TABLE Usuarios (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    data_registro DATE NOT NULL
);

CREATE TABLE Artistas (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    data_nascimento DATE
);

CREATE TABLE Discos (
    id SERIAL PRIMARY KEY,
    titulo VARCHAR(255) NOT NULL,
    data_lancamento DATE,
    artista_id INT NOT NULL,
    FOREIGN KEY (artista_id) REFERENCES Artistas(id)
);

CREATE TABLE Musicas (
    id SERIAL PRIMARY KEY,
    titulo VARCHAR(255) NOT NULL,
    duracao INT NOT NULL,
    disco_id INT NOT NULL,
    FOREIGN KEY (disco_id) REFERENCES Discos(id)
);

CREATE TABLE Playlists (
    id SERIAL PRIMARY KEY,
    titulo VARCHAR(255) NOT NULL,
    usuario_id INT NOT NULL,
    FOREIGN KEY (usuario_id) REFERENCES Usuarios(id)
);

CREATE TABLE Playlist_Musicas (
    playlist_id INT NOT NULL,
    musica_id INT NOT NULL,
    PRIMARY KEY (playlist_id, musica_id),
    FOREIGN KEY (playlist_id) REFERENCES Playlists(id),
    FOREIGN KEY (musica_id) REFERENCES Musicas(id)
);

CREATE TABLE Musica_Artistas (
    musica_id INT NOT NULL,
    artista_id INT NOT NULL,
    PRIMARY KEY (musica_id, artista_id),
    FOREIGN KEY (musica_id) REFERENCES Musicas(id),
    FOREIGN KEY (artista_id) REFERENCES Artistas(id)
);
```
### Instruções:
#### 1. **Rodar o Código SQL:**
   - Copie e cole o código SQL no console ou no editor de SQL.
   - O código cria as seguintes tabelas:
     - **Usuarios**: Armazena dados dos usuários.
     - **Artistas**: Armazena informações dos artistas.
     - **Discos**: Guarda os discos lançados pelos artistas.
     - **Musicas**: Armazena as músicas e suas informações.
     - **Playlists**: Contém as playlists criadas pelos usuários.
     - **Playlist_Musicas**: Liga músicas às playlists (relação N:N).
     - **Musica_Artistas**: Liga músicas aos artistas (relação N:N).

#### 2. **Executar o Código SQL:**
   - Cole o código diretamente e aperte **Enter** ou **Executar**

#### 3. **Verificar se as Tabelas Foram Criadas**:
   - Após a execução bem-sucedida, verifique se as tabelas foram criadas corretamente.

#### 4. **Inserir Dados nas Tabelas**:
   - Depois que as tabelas são criadas, insira dados como este exemplo:
     ```sql
     INSERT INTO Usuarios (nome, email, data_registro) VALUES ('João Silva', 'joao@exemplo.com', '2024-01-01');
     ```

Agora o banco de dados está pronto para armazenar informações do seu sistema de streaming de música!


## Código desenvolvido para gerar dados aleatórios:
```
from faker import Faker
import random
import mysql.connector

fake = Faker()

db_config = {
    'host': 'localhost',
    'user': 'usuario',
    'password': 'senha',
    'database': 'streaming_musica'
}

db = mysql.connector.connect(**db_config)
cursor = db.cursor()

def gerar_dados():
    for _ in range(10):
        nome = fake.name()
        data_nascimento = fake.date_of_birth()
        cursor.execute("INSERT INTO Artistas (nome, data_nascimento) VALUES (%s, %s)", (nome, data_nascimento))

    for _ in range(10):
        nome = fake.name()
        email = fake.email()
        data_registro = fake.date_this_year()
        cursor.execute("INSERT INTO Usuarios (nome, email, data_registro) VALUES (%s, %s, %s)", (nome, email, data_registro))

    for _ in range(10):
        titulo_disco = fake.sentence(nb_words=3)
        data_lancamento = fake.date()
        artista_id = random.randint(1, 10)
        cursor.execute("INSERT INTO Discos (titulo, data_lancamento, artista_id) VALUES (%s, %s, %s)", (titulo_disco, data_lancamento, artista_id))
        disco_id = cursor.lastrowid

        for _ in range(random.randint(5, 10)):
            titulo_musica = fake.sentence(nb_words=2)
            duracao = random.randint(120, 360)
            cursor.execute("INSERT INTO Musicas (titulo, duracao, disco_id) VALUES (%s, %s, %s)", (titulo_musica, duracao, disco_id))

    for _ in range(10):
        titulo_playlist = fake.sentence(nb_words=2)
        usuario_id = random.randint(1, 10)
        cursor.execute("INSERT INTO Playlists (titulo, usuario_id) VALUES (%s, %s)", (titulo_playlist, usuario_id))
        playlist_id = cursor.lastrowid

        for _ in range(random.randint(5, 10)):
            musica_id = random.randint(1, 50)  # Assume que existem até 50 músicas inseridas
            cursor.execute("INSERT INTO Playlist_Musicas (playlist_id, musica_id) VALUES (%s, %s)", (playlist_id, musica_id))

    db.commit()

if __name__ == "__main__":
    try:
        gerar_dados()
        print("Dados inseridos com sucesso!")
    except mysql.connector.Error as err:
        print(f"Erro: {err}")
        db.rollback()
    finally:
        cursor.close()
        db.close()
```
### Instruções:
#### 1. **Pré-requisitos:**
   - **Python 3** instalado.
   - Biblioteca **Faker** instalada:
     - Execute no terminal:
       ```python
       pip install faker
       ```
   - Biblioteca **mysql-connector-python** instalada:
     - Execute no terminal:
       ```python
       pip install mysql-connector-python
       ```
   - Banco de dados MySQL criado e configurado.

#### 2. **Configurar o Banco de Dados:**
   - Crie um banco de dados MySQL chamado `streaming_musica`.
   - Certifique-se de que as tabelas estão criadas conforme o código SQL anterior.

#### 3. **Configurar Conexão com o Banco de Dados:**
   - No código, altere as configurações de conexão com o banco:
     ```python
     db_config = {
         'host': 'localhost',
         'user': 'usuario',   # Substitua por seu usuário do MySQL
         'password': 'senha', # Substitua pela sua senha
         'database': 'streaming_musica'
     }
     ```
   - Ajuste `user` e `password` conforme as credenciais do seu banco de dados.

#### 4. **Gerar e Inserir Dados Aleatórios:**
   - O código gera dados aleatórios para as seguintes tabelas:
     - **Artistas**: 10 artistas com nomes e datas de nascimento.
     - **Usuarios**: 10 usuários com nomes, e-mails e datas de registro.
     - **Discos**: 10 discos, cada um associado a um artista.
     - **Musicas**: Cada disco tem entre 5 e 10 músicas.
     - **Playlists**: 10 playlists, cada uma associada a um usuário.
     - **Playlist_Musicas**: Cada playlist tem entre 5 e 10 músicas aleatórias.

#### 5. **Rodar o Código Python:**
   - Salve o código Python em um arquivo (`inserir_dados.py`, por exemplo).
   - Execute o arquivo no terminal:
     ```python
     python inserir_dados.py
     ```
   - O código conectará ao banco de dados e gerará os dados automaticamente.

#### 6. **Verificar Inserções no Banco de Dados:**
   - Após a execução, os dados aleatórios serão inseridos nas tabelas.
   - Use uma ferramenta como **MySQL Workbench** ou o terminal **MySQL** para verificar os dados:
     ```sql
     SELECT * FROM Artistas;
     SELECT * FROM Usuarios;
     ```