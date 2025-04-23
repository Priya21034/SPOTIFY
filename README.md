# SPOTIFY SQL POSTGRE

Objective: 
This project involves analysing a Spotify dataset with various attributes about tracks, albums, and artists using SQL. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.

** create table
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);

 ** Q.1 Retrieve the names of all tracks that have more than 1 billion streams.
 
 SELECT * FROM SPOTIFY
WHERE stream >1000000000;

** Q.2 List all albums along with their respective artists.

     SELECT ALBUM,ARTIST 
      FROM SPOTIFY
       ORDER BY  1;
SELECT Distinct ALBUM,ARTIST
    FROM SPOTIFY
    ORDER BY 1;

** Q.3 Get the total number of comments for tracks where licensed = TRUE.

      SELECT SUM(COMMENTS)AS TOTAL_COMMENTS
        FROM SPOTIFY
        WHERE LICENSED='TRUE';

** Q.4 Find all tracks that belong to the album type single.

      SELECT * FROM SPOTIFY  
      WHERE album_type ='single';

** Q.5 Count the total number of tracks by each artist.

      SELECT ARTIST,COUNT(*) as total_no_songs
      FROM SPOTIFY
      GROUP BY  Artist
      order by 2 ;
      
** Q6. Calculate the average danceability of tracks in each album.

       SELECT album, avg(danceability) as avg_dancebility
       FROM SPOTIFY
       GROUP BY 1
       ORDER BY 2 DESC;

** Q.7. Find the top 5 tracks with the highest energy values.

        SELECT TRACK, MAX(ENERGY) AS "Avg_energy"
        FROM SPOTIFY
        Group by 1
        ORDER BY 2 DESC
        LIMIT 5;

**Q.8 List all tracks along with their views and likes where official_video = TRUE.

      SELECT track,
      SUM(Views) AS Total_Views,
      SUM(likes) AS Total_likes
      FROM SPOTIFY
      WHERE OFFICIAL_VIDEO=TRUE
      GROUP BY 1
      ORDER BY 2 DESC;

** Q.9 For each album, calculate the total views of all associated tracks.

    SELECT ALBUM,track,
    SUM(Views) AS Total_Views
    FROM SPOTIFY
    GROUP BY 1,2
    ORDER BY 2 DESC;

** Q.10 Retrieve the track names that have been streamed on Spotify more than YouTube.

       SELECT * FROM 
    (SELECT 
     track,
            --- most_played_on,
            COALESCE(SUM(CASE WHEN MOST_PLAYED_ON ='Youtube' THEN stream END),0) AS STREAMED_ON_YOUTUBE,
            COALESCE(SUM(CASE WHEN MOST_PLAYED_ON ='spotify' THEN stream END),0) AS STREAMED_ON_SPOTIFY
      FROM spotify
      GROUP BY 1
      ) as t1
      WHERE 
      STREAMED_ON_SPOTIFY > STREAMED_ON_YOUTUBE
      AND 
	  STREAMED_ON_YOUTUBE <> 0;

** Q11.Find the top 3 most-viewed tracks for each artist using window functions.

       WITH ranking_artist
       AS (SELECT artist,track,
            SUM(VIEWS) AS  "total_view",
			DENSE_RANK() OVER(PARTITION BY artist ORDER BY SUM(VIEWS)DESC) AS RANK 
       FROM spotify
       GROUP BY 1,2
       ORDER BY 1,3 DESC 
       )
       SELECT * FROM ranking_artist
       WHERE Rank <= 3;

** Q.12. Write a query to find tracks where the liveness score is above the average.

         SELECT 
             artist,
	     track,
	     liveness
	     From spotify 
         WHERE Liveness > (SELECT AVG (liveness) FROM SPOTIFY)

** Q.13.Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.

         WITH CTE
         AS 
         (SELECT
		album,
		MAX(Energy) as highest_energy,
		MIN(Energy) as lowest_energy
         FROM SPOTIFY
         GROUP BY 1)
         SELECT 
         album,
	 highest_energy-lowest_energy as energy_diff
	 FROM cte
	 ORDER BY 2 DESC;

  
  
 -<a href = "https://github.com/Priya21034/SPOTIFY/blob/main/README.md"> View Dashboard </a>
 


![SQL-PNG1](https://github.com/user-attachments/assets/ea37bfe0-e8c6-41d6-992f-2013941761a4)


      
![SQL-PNG2](https://github.com/user-attachments/assets/51aff47f-1841-4417-a6a6-73b982c5fbf2)



![SQL-PNG3](https://github.com/user-attachments/assets/dc2b9cba-4ccf-4eaf-9d0b-2aecb885aae9)












