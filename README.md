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