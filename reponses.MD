On peut afficher avec SELECT * FROM nom_table, et aussi avoir plus d'informations avec
sp_help 'nom_table'

tArtist: idArtist (clé primaire), primaryName, birthYear

tFilm: idFilm (clé primaire), primaryTitle, startYear, averageRating, runtimeMinutes

tFilmGenre: idFilm (clé primaire), idGenre (clé étrangère)

tGenre: idGenre (clé primaire), genre
Cette table est vide

tJob: idArtist (clé étrangère), category, idFilm (clé étrangère)

SELECT birthYear
FROM tArtist
WHERE primaryName = 'Jude Law';

SELECT COUNT(*)
FROM tArtist;

SELECT primaryName
FROM tArtist
WHERE birthYear = 1960;

SELECT COUNT(*) AS NombreArtistes
FROM tArtist
WHERE birthYear = 1960;

SELECT TOP 1 birthYear, COUNT(*) AS NombreActeurs
FROM tArtist a
JOIN tJob j ON a.idArtist = j.idArtist
WHERE j.category = 'acted in'
AND a.birthYear <> 0
GROUP BY a.birthYear
ORDER BY COUNT(*) DESC;

SELECT a.primaryName
FROM tArtist a
JOIN tJob j ON a.idArtist = j.idArtist
WHERE j.category = 'acted in'
GROUP BY a.primaryName
HAVING COUNT(DISTINCT j.idFilm) > 1;

SELECT a.primaryName
FROM tArtist a
JOIN tJob j ON a.idArtist = j.idArtist
GROUP BY a.idArtist, a.primaryName
HAVING COUNT(DISTINCT j.category) > 1;

SELECT f.primaryTitle, COUNT(DISTINCT j.idArtist) AS NombreActeurs
FROM tFilm f
JOIN tJob j ON f.idFilm = j.idFilm
WHERE j.category = 'acted in'
GROUP BY f.idFilm, f.primaryTitle
ORDER BY NombreActeurs DESC;

On ne compte pas si un acteur joue deux fois dans le film

SELECT a.primaryName, f.primaryTitle
FROM tArtist a
JOIN tJob j ON a.idArtist = j.idArtist
JOIN tFilm f ON j.idFilm = f.idFilm
GROUP BY a.primaryName, f.primaryTitle
HAVING COUNT(DISTINCT j.category) > 1;

