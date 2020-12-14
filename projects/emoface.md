---
title: EmoFace
layout: page
---

# EmoFace

Making immersiveness easier -> EmotionTracker is a tool combining privacy and state-of-the-art machine learning, that will enable and incentivize the creation about a lot of innovative web apps by providing developers with an easy way of accessing the user's mood and emotions, without being concerned about privacy.

## emotion-tracker-js

A js wrapper for our Emotion Recognition engine. Gives developers easy access to the user's emotion.

### How to use

It all revolves around the `EmotionTracker` class.

First install this library

```bash
npm install 'emotion-tracker-js'
```

Import and initialize our tracker!

```javascript
import EmotionTracker from "emotion-tracker-js";

let emotionUpdateCallback = () => {};

let emotionTracker = new EmotionTracker(emotionUpdateCallback);
emotionTracker.track();
// You need to take the pictures and establish the connection with the webcam and use that
// ... Webcam code that takes picture
let emotion = emotionTracker.poll(imageFile);
// As soon as the API request is resolved, the callback you specified will be called '
// and you will have access to the newest emotional state of the user. You can use the variable emotion as well.
```

### Callbacks and State Management

Our module makes use of javascript proxies thanks to which the user will be notified each time the user's emotion changes.
Developers can easily abstract their functionality into a callback which accepts the following arguments

```javascript
function myCallback(key, value, target) {
  // do whatever you want with these
}
```

The callback is invoked with the parameters of the property that is changed in the emotion state object as well as the value it is taking and the previous state value.

## Movie recommendation API

Movie recommendation API operates as a recommendation API that based on the provided emotion recommends movies of some genre.

### Dataset

It internally uses IMDB dataset and recommends top IMDB movies. To setup a recommendation API, the IMDB dataset must be downloaded. Cleaned IMDB dataset in CSV format can be found [`here`](https://drive.google.com/file/d/1Sb6UVwp6HYb7huo74Kbf-XDqQsM334rV/view?usp=sharing). To use it properly, place it into the **_movie-recommendation-api/data_** directory. The dataset contains data for more than 85k IMDB movies.

### Installation

API uses SQLite DBMS, so make sure you install it. API is developed with the Flask framework. Dependencies can be found in requirements.txt file and can be downloaded from PyPi.

```python
pip install <dependency_module>
```

The easier way to install them is to let pip install all the requirements modules on itself:

```python
pip install -r requirements.txt
```

Another option is to use poetry config file to install all dependencies:

```python
poetry install
```

### Usage

First of all, database must be created and populated.

```python
python3 load_db.py
```

This initial database dumping can take a while (to insert more than 85k records).
After you've loaded the database and installed all the requirement modules, you can run the recommendation API server.

```python
python3 main.py
```

The server will go live on port 8000.

### Endoints

The only avaiable endpoint is the recommendation endpoint with the given URL: /api/v1/recommend\
HTTP method: POST\
Consumes: application/json\
Produces: application/json\
Request body:

```json
{
  "emotion": "emotion that should determine genre of the recommended movies",
  "num_of_movies": 5,
  "recommendation_type": "type of recommendation you want"
}
```

Request body parameters description:

```
*emotion* - type := string ; possible values := {"Angry","Disguist", "Fear", "Happy", "Sad","Fantasy", "Neutral"}
*num_of_movies* - type := int (positive) e.g. 10
*recommendation_type* - type := string; possible values := {"improve", "keepup"}
```

### Example

- Request example:

```bash
curl --location --request POST 'localhost:8000/api/v1/recommend' --header 'Content-Type: application/json' --data-raw '{"emotion": "Happy", "num_of_movies": 3, "recommendation_type": "improve"}'
```

- Response example:

```json
{
  "movies": [
    {
      "actors": "Ron Moody, Mike Reid, Miles Petit, Danny Ogle, Jason Gerard, Helena Roman, Matthew Hendrickson, Spyros Merianos, Abbie Balchin, Rachel Balchin, Jason Bullet, Ernesto Cantu, Andy Cheeseman, Emily Corcoran, Lynette Creane",
      "description": "Getaway driver Miles Foster is placed in witness protection after the murder of his friend Andres by Astin Brody, a shady underworld boss. Miles is hidden on the Greek Island of Zanthi with...",
      "directors": "Danny Patrick",
      "duration": 90,
      "genre_name": "Adventure",
      "imdb_url": "https://www.imdb.com/title/tt0451130/",
      "movie_id": "tt0451130",
      "poster_url": "https://m.media-amazon.com/images/M/MV5BOTUwMmI1MWEtYzRkNy00NjBkLTllM2YtZmNkNmVjODI1M2Q2XkEyXkFqcGdeQXVyMzA2ODY0NA@@.jpg",
      "production_company": "Empire Productions",
      "score": 9.3,
      "title": "Moussaka & Chips",
      "year": 2005
    },
    {
      "actors": "Bryan Cranston, Arun Govil, Edie Mirman, Rael Padamsee, Namrata Sawhney, James Earl Jones, Shatrughan Sinha, Jinder Walia, Amrish Puri, Mishal Varma, Tom Wyner, Richard Cansino, Shakti Singh, Dilip Sinha, Michael Sorich",
      "description": "An anime adaptation of the Hindu epic the Ramayana, where Lord Ram combats the wicked king Ravana.",
      "directors": "Ram Mohan, Yûgô Sakô",
      "duration": 170,
      "genre_name": "Adventure",
      "imdb_url": "https://www.imdb.com/title/tt0259534/",
      "movie_id": "tt0259534",
      "poster_url": "https://m.media-amazon.com/images/M/MV5BOTk4NGM0NmUtOTc2Yi00NTcxLWE3NGItYzEwODZjNzlhZjE2XkEyXkFqcGdeQXVyNTgyNTA4MjM@.jpg",
      "production_company": "Nippon Ramayana Film Co.",
      "score": 9,
      "title": "Ramayana: The Legend of Prince Rama",
      "year": 1992
    },
    {
      "actors": "",
      "description": "",
      "directors": "Yukio Kaizawa",
      "duration": 70,
      "genre_name": "Adventure",
      "imdb_url": "https://www.imdb.com/title/tt10037700/",
      "movie_id": "tt10037700",
      "poster_url": "https://m.media-amazon.com/images/M/MV5BYzIyMWRkMTktZmYyYy00YzZkLWFlOWEtOGU2MmEyNjVkZDk3XkEyXkFqcGdeQXVyNzEyMDQ1MDA@.jpg",
      "production_company": "Toei Animation",
      "score": 9,
      "title": "Precure Miracle Universe Movie",
      "year": 2019
    }
  ]
}
```

## Pod 1.0.0 - Team 1

- Viktor Velev
- Nenad Pantelić
- Temitope Akinsoto

## Tech stack

- Programming languages: Python, JavaScript,
- Frameworks: BentoML, Flask, React, SQLAlchemy
- UI: HTML, CSS, Ant Design
- VCS: git and GitHub

# License

This project is under the [MIT License](https://en.wikipedia.org/wiki/MIT_License)

# Link

Github: [EmoFace](https://github.com/VikVelev/EmotionTracker)
