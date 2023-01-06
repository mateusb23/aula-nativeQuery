# Busca Simples e Join Simples

### Base de dados

```
genres(id: integer, name: string);

artists(id: integer, name: string, genre_id: integer, birthdate: date, contry: string);
  genre_id references genres
```

### Script criação das tabelas

```sql
CREATE TABLE genres (
  id             INTEGER     NOT NULL,
  name           VARCHAR(50) NOT NULL,
  PRIMARY KEY(id)
);

CREATE TABLE artists (
  id             INTEGER      NOT NULL,
  name           VARCHAR(100) NOT NULL,
  genre_id       INTEGER,
  birthdate      DATE,
  country        VARCHAR(30),	
  PRIMARY KEY(id),
  FOREIGN KEY(genre_id) REFERENCES genres(id)
);
```

### Seeding do banco de dados

```sql

CREATE TABLE genres(
	id INTEGER NOT NULL,
	name VARCHAR(50) NOT NULL,
	PRIMARY KEY(id)
);

CREATE TABLE artists(
	id INTEGER NOT NULL,
	name VARCHAR(50) NOT NULL,
	genre_id INTEGER,
	birthdate DATE,
	country VARCHAR(30),
	PRIMARY KEY(id),
	FOREIGN KEY(genre_id) REFERENCES genres(id)
);

INSERT INTO genres(id,name) VALUES(1, 'Pop');
INSERT INTO genres(id,name) VALUES(2, 'Rock');
INSERT INTO genres(id,name) VALUES(3, 'Heavy Metal');
INSERT INTO genres(id,name) VALUES(4, 'Mpb');
INSERT INTO genres(id,name) VALUES(5, 'Rap');
INSERT INTO genres(id,name) VALUES(6, 'Eletrônica');
INSERT INTO genres(id,name) VALUES(7, 'Hip Hop');
INSERT INTO genres(id,name) VALUES(8, 'R&B');
INSERT INTO genres(id,name) VALUES(9, 'Samba');


INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(1, 'Beyoncé', 1, '1981-09-04', 'United States');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(2, 'Nina Simone', 8, '1933-02-21', 'United States');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(3, 'Caetano Veloso', 4, '1942-08-07', 'Brazil');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(4, 'Milton Nascimento', 4, '1942-10-26', 'Brazil');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(5, 'Elis Regina', 4, '1945-03-17', 'Brazil');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(6, 'Lady Gaga', 1, '1986-03-28', 'United States');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(7, 'Adele', 1, '1988-05-05', 'United Kingdom');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(8, 'Shakira', 1, '1977-02-02', 'Colombia');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(9, 'Anitta', 1, '1993-03-30', 'Brazil');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(10, 'Cartola', 9, '1908-11-11', 'Brazil');
INSERT INTO artists(id,name,genre_id,birthdate,country) VALUES(11, 'Ivone Lara', 9, '1921-04-13', 'Brazil');

SELECT * FROM genres;
SELECT * FROM artists;

SELECT artists.id AS "artists_id",
	   genres.name AS "Genre",
	   artists.birthdate AS "Birthdate",
	   artists.country AS "Country"
	   FROM genres
	   JOIN artists ON artists.genre_id = genres.id;

```

## Estudos de caso

### busca simples: 
Exibir o nome de todos os artistas cujo país seja 'Brazil'

#### Consulta

```sql
SELECT name 
FROM artists 
WHERE country LIKE 'Brazil'
```

#### Projection

```java
public interface ArtistMinProjection {

	String getName();
}
```

#### Repository

```java
@Query(nativeQuery = true, value = "SELECT name " + 
	"FROM artists " + 
	"WHERE UPPER(country) LIKE UPPER(:country)")

List<ArtistMinProjection> findByCountry(String country);
```

### JOIN simples: 
Exibir o nome e a nacionalidade dos artista cujo gênero seja 'Pop'

#### Consulta

```sql
SELECT artists.name, artists.country 
FROM artists
INNER JOIN genres ON artists.genre_id = genres.id
WHERE genres.name = 'Pop'
```

#### Projection

```java
public interface ArtistJoinMinProjection {

	String getName();
	String getCountry();
}
```

#### Repository

```java
@Query(nativeQuery = true, value = "SELECT artists.name, artists.country " + 
	"FROM artists " + 
	"INNER JOIN genres ON artists.genre_id = genres.id " + 
  "WHERE UPPER(genres.name) = UPPER(:genreName)")

List<ArtistJoinMinProjection> findByGenreJoin(String genreName);
```



