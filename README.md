# Learning-Spotipy
This python script establishes a Spotipy client, which engages with Spotify's public data through a python API. It establishes a class including several functions, including a generic Search as well as specific get methods, for artists, albums, and related artists. The methods are used to retrieve information about artists having song titles with the same name, e.g., "When the Levee Breaks". It graphs the results in treemaps and rooted tree exhibits, using follower count as a numerical scale. Finally it retrieves related artists, as determined by Spotify.

## Usage
First, we must register an application with Spotify through the developer dashboard here: https://developer.spotify.com/. Register a client_id and a client_secret to use for your application. These credentials allow the API to read and write. Next, we establish our SpotifyAPI class, which will hold all our functions.

The get_client_credentials() method sends our client_id and client_secret keys to Spotify for verification. Having received confirmation they are approved credentials, it decodes encoded bytes. These decoded bytes allow us to run the perform_auth() method and request a token, which lets us call our other functions.

![spotipy_class1](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/spotipy_class1.png?raw=true)

Spotify provides extensive documentation to engage with its API: https://developer.spotify.com/documentation/. Our get_resource(), get_access_token(), and other methods match this syntax to read and write json elements from the API.

![spotipy_class2](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/spotipy_class2.png?raw=true)

The get_resource() method works as a vehicle for the search(), get_artist(), and get_album() methods. Those methods call get_resource() to pass queries to the API with the proper endpoint and headers.

![spotipy_class3](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/spotipy_class3.png?raw=true)

The search() method passes our query, such as a song title string "When the Levee Breaks", to the API and returns a json string carrying data back from Spotify. The get_artist() and get_album() methods pass an id, either artist_id or album_id, and return a json element from Spotify. The get_related_artists() method performs similarly, but has a slightly different endpoint (url).

![spotipy_class4](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/spotipy_class4.png?raw=true)

We want to find artists who have a track titled "When the Levee Breaks." We run a search with search_type='track' and get back json, which we unpack to show all the matching artists. We perform a few more queries to get information such as follower count and genres tagged to each artist. We store all this data in a dict for easy access.

![query1](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/query1.png?raw=true)

We want to see which genres were most frequent in our artists returned. To accomplish this, we walk into each artist's genre list and concatenate to a single list, which we dedupe. Then we check whether each genre is present in each artists key, building a frequency table (effectively a histogram).

![unique_genres](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/unique_genres.png?raw=true)

The next step is to choose a primary genre for each artist. Our rudimentary method is to choose the first alphabetically. We place this alongside the follower count for each artist, so as to graph in treemaps.

![artists_primary_genres](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/artists_primary_genres.png?raw=true)

We plot our results in several treemaps, the most informative of which is organized with genres followed by artists. The most common genre was "album rock" and was comprised by three artist: Led Zeppelin, Great White, and Sammy Hagar.

![treemapLeveeBreaks](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Exhibits/treemapLeveeBreaks.png?raw=true)

How many of these artists are playing the same song, many as covers? Or is it a coincidence that the tracks have the same name? While it isn't likely coincidence for "When the Levee Breaks" it could easily be for tracks named more generically, such as "Love Song." We can visualize the interconnectedness of our artists by finding each's related artists, according to Spotify.

We run our get_related_artists() method for a few of the artists we discovered (e.g., Led Zeppelin, Silversun Pickups, and Whiskey Myers). Before we can graph each set, we need to retrieve artist info, such as follower count, for all the artists returned. Then we transform into clean DataFrames.

![related_artists1](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/related_artists1.png?raw=true)

We are going to plot this in a Scatter object using matplotlib. To format the Scatter, we transform our artist and follower count pandas Series into numpy arrays.

![related_artists2](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/related_artists2.png?raw=true)
![related_artists3](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/related_artists3.png?raw=true)

Then we establish a Figure object and format it for readability, adding titles, specifying font sizes, and adding data labels for each scatter marker.

![related_artists4](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/related_artists4.png?raw=true)
![related_artists5](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Code/related_artists5.png?raw=true)

Led Zeppelin and Whiskey Myers each have more followers than all of their related artists, while Silversun Pickups have fewer than the majority of their peers. Our three example artists have very different related artist lists, sitting in very different genres.

![relatedartists_LedZeppelin](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Exhibits/relatedartists_LedZeppelin.png?raw=true)

![relatedartists_SilversunPickups](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Exhibits/relatedartists_SilversunPickups.png?raw=true)

![relatedartists_WhiskeyMyers](https://github.com/drussel4/Learning-Spotipy/blob/master/img/Exhibits/relatedartists_WhiskeyMyers.png?raw=true)

And that wraps it up! This is a surface level implementation of Spotify. It can be used to create full scale applications and can even be configured to engage with user level data, though that requires another level of privacy customization (since user data is protected). But this project should give a taste of the uses we can apply Spotipy towards!

## License

MIT © Dave Russell
