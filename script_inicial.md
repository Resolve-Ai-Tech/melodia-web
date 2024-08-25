## Script Inicial

### Instalação do PostgreSQL

Se você ainda não tem o PostgreSQL instalado, siga os passos abaixo:

#### No Windows:

1. [Baixe o PostgreSQL 16](https://sbp.enterprisedb.com/getfile.jsp?fileid=1259129) e execute o instalador.
2. Durante a instalação, configure a senha do superusuário (`postgres`).
3. Finalize a instalação e abra o pgAdmin para gerenciar o banco de dados.

### Configuração do Banco de Dados

1. Conecte-se ao PostgreSQL usando psql, pgAdmin, DBeaver ou outra ferramenta de sua preferência.
2. Crie um banco de dados chamado `melodia`.
3. Conecte-se ao banco de dados `melodia`.
4. Execute os seguintes comandos SQL para criar as tabelas necessárias e popular o banco com dados fictícios:
```sql
-- Criação das tabelas
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    senha VARCHAR(255) NOT NULL,
    nome VARCHAR(255) NOT NULL
);

CREATE TABLE musicas (
    id SERIAL PRIMARY KEY,
    titulo VARCHAR(255) NOT NULL,
    artista VARCHAR(255) NOT NULL,
    album VARCHAR(255),
    genero VARCHAR(50),
    duracao INT NOT NULL,
    capaUrl VARCHAR(255)
);

CREATE TABLE playlists (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    descricao TEXT,
    usuario_id INT REFERENCES usuarios(id) ON DELETE CASCADE
);

CREATE TABLE playlist_musicas (
    playlist_id INT REFERENCES playlists(id) ON DELETE CASCADE,
    musica_id INT REFERENCES musicas(id) ON DELETE CASCADE,
    PRIMARY KEY (playlist_id, musica_id)
);

CREATE TABLE avaliacoes_musicas (
    id SERIAL PRIMARY KEY,
    musica_id INT REFERENCES musicas(id) ON DELETE CASCADE,
    usuario_id INT REFERENCES usuarios(id) ON DELETE CASCADE,
    avaliacao TEXT NOT NULL,
    sentimento VARCHAR(50) NOT NULL
);

CREATE TABLE atividades_usuarios (
    id SERIAL PRIMARY KEY,
    usuario_id INT REFERENCES usuarios(id) ON DELETE CASCADE,
    atividade VARCHAR(50) NOT NULL,
    data_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE historico_uso (
    id SERIAL PRIMARY KEY,
    usuario_id INT REFERENCES usuarios(id) ON DELETE CASCADE,
    musica_id INT REFERENCES musicas(id) ON DELETE CASCADE,
    data_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    acao VARCHAR(50) NOT NULL  -- Ex: 'play', 'pause', 'skip', etc.
);

CREATE TABLE tendencias_musicais (
    id SERIAL PRIMARY KEY,
    musica_id INT REFERENCES musicas(id) ON DELETE CASCADE,
    contagem_reproducoes INT NOT NULL,
    periodo VARCHAR(50) NOT NULL,  -- Ex: 'diario', 'semanal', 'mensal'
    data_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE preferencias_usuarios (
    id SERIAL PRIMARY KEY,
    usuario_id INT REFERENCES usuarios(id) ON DELETE CASCADE,
    preferencia VARCHAR(50) NOT NULL,  -- Ex: 'treino', 'relaxamento', 'estudo'
    genero_preferido VARCHAR(50),  -- Ex: 'Pop', 'Rock', 'Jazz'
    data_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Usuários
INSERT INTO usuarios (email, senha, nome) VALUES
('alice.smith@example.com', 'hashed_password1', 'Alice Smith'),
('bob.jones@example.com', 'hashed_password2', 'Bob Jones'),
('carla.garcia@example.com', 'hashed_password3', 'Carla Garcia');

-- Músicas
INSERT INTO musicas (titulo, artista, album, genero, duracao, capaUrl) VALUES
('Blinding Lights', 'The Weeknd', 'After Hours', 'Pop', 200, 'http://example.com/covers/blinding_lights.jpg'),
('Watermelon Sugar', 'Harry Styles', 'Fine Line', 'Pop', 174, 'http://example.com/covers/watermelon_sugar.jpg'),
('Levitating', 'Dua Lipa', 'Future Nostalgia', 'Pop', 203, 'http://example.com/covers/levitating.jpg'),
('Shape of You', 'Ed Sheeran', 'Divide', 'Pop', 233, 'http://example.com/covers/shape_of_you.jpg'),
('Peaches', 'Justin Bieber', 'Justice', 'R&B', 198, 'http://example.com/covers/peaches.jpg'),
('Rockstar', 'DaBaby', 'Blame It on Baby', 'Hip-Hop', 181, 'http://example.com/covers/rockstar.jpg');

-- Playlists
INSERT INTO playlists (nome, descricao, usuario_id) VALUES
('Top Hits 2024', 'The best songs of 2024', 1),
('Relaxing Vibes', 'Chill music to relax and unwind', 2),
('Workout Mix', 'High-energy tracks to keep you motivated during your workout', 3);

-- Associações de músicas em playlists
INSERT INTO playlist_musicas (playlist_id, musica_id) VALUES
(1, 1),
(1, 2),
(2, 3),
(2, 5),
(3, 4),
(3, 6);

-- Avaliações de músicas
INSERT INTO avaliacoes_musicas (musica_id, usuario_id, avaliacao, sentimento) VALUES
(1, 1, 'This song is an absolute banger!', 'Positivo'),
(2, 2, 'Nice summer vibe!', 'Positivo'),
(3, 3, 'Love the rhythm, but lyrics could be better.', 'Neutro'),
(4, 1, 'This track never gets old.', 'Positivo'),
(5, 2, 'Good, but not great.', 'Neutro'),
(6, 3, 'Perfect for a workout session!', 'Positivo');

-- Atividades dos usuários
INSERT INTO atividades_usuarios (usuario_id, atividade) VALUES
(1, 'estudo'),
(2, 'relaxamento'),
(3, 'treino');

-- Dados iniciais para histórico de uso
INSERT INTO historico_uso (usuario_id, musica_id, acao) VALUES
(1, 1, 'play'),
(1, 2, 'pause'),
(2, 3, 'play'),
(3, 4, 'skip'),
(3, 6, 'play');

-- Dados iniciais para tendências musicais
INSERT INTO tendencias_musicais (musica_id, contagem_reproducoes, periodo) VALUES
(1, 500, 'semanal'),
(2, 450, 'semanal'),
(3, 300, 'semanal'),
(4, 700, 'semanal'),
(5, 200, 'semanal'),
(6, 350, 'semanal');

-- Dados iniciais para preferências de usuários
INSERT INTO preferencias_usuarios (usuario_id, preferencia, genero_preferido) VALUES
(1, 'estudo', 'Jazz'),
(2, 'relaxamento', 'Pop'),
(3, 'treino', 'Hip-Hop');
`````
