### Developer Note:

This is my first personal project while learning Python. I am in the early 
stages and will work on this as I learn new things in Python. 

### Background:

I'm a Spotify user and love to see what new releases come out every Friday.
Right now, I have to manually open the app, click on Browse, click on New
Releases, and then scroll through the list. 

I would rather get an email with all New Releases every Friday, which is what
this project is intended for. 

### End Goal: 
Have a fully automated program which sends me an email every Friday with 
Spotify's New Releases.  The email will include the album art along with the artist 
name(s) and song/album title.  The album art will be a clickable link 
which opens Spotify. 


### Current Implementation:
- A request is made to the Spotify API with a specified user account
 to get the 40 newest releases in the USA. 
- The json is parsed for singles, albums and their artists.
- The artist is grouped under the release.
- A plain text email gets sent using gmail smtp to a specified user. 

Example output: 
```
New Music:

SINGLE: The Bigger Picture
by Lil Baby

SINGLE: Make It Rain
by Pop Smoke
by Rowdy Rebel

ALBUM: Ungodly Hour
by Chloe x Halle

...
```

### Future Goals:
- Make the email pretty and provide album art along with artist/song/album.
- Make the album art in the email clickable so it opens Spotify and plays music. 

### Completed Goals:
- Figure out how to group artists with their album.  As of today, i am able to 
print the singles/albums list first followed by an artist list. This is troublesome
as I don't know which single/album belongs to which artist.
- Figure out how to group artists if they belong to the same single/album.
- Figure out how to get a token for Spotify API as the one provided in
the test API expires. (https://developer.spotify.com/documentation/general/guides/authorization-guide/)
- Look into Spotipy for making api call instead of writing my own 
GET (https://spotipy.readthedocs.io/) 
- Figure out how to save output to local file which will be used as the email
body later on.
- Figure out how to send an email automatically using Python using a txt file
as the body

## HOW-TO
First and foremost, you will need a Spotify account. If you do not have one, go sign up.
1. Head to [Spotify Develop Dashboard](https://developer.spotify.com/dashboard/applications)
and create a new app. 
    1. Make note of your client ID and client secret.
    2. Click on the app and then Edit Settings. 
        1. add `http://127.0.0.1/` for Redirect URIs and save.
2. Clone this repo locally.
3. Install dependencies
    - [Spotipy](https://spotipy.readthedocs.io/en/2.7.0/#installation)
    - If you are using venv, you can use the provided requirements.txt.
    - OR
    - You can also install spotipy via pip3.  Note: Results may vary based on dependency version.
    ```
    pip3 install spotipy --upgrade
   ```
4. Add environment variables via Terminal.
    - macOS 10.5 and higher:
    1. You can permanently add variables to ~/.zshenv.
    ```
    echo 'export SPOTIPY_CLIENT_ID='your-spotify-client-id'' >> ~/.zshenv
    echo 'export SPOTIPY_CLIENT_SECRET='your-spotify-client-secret'' >> ~/.zshenv
    echo 'export SPOTIPY_REDIRECT_URI='http://127.0.0.1/'' >> ~/.zshenv
    ```
   3. Linux/bash: 
   ```
    echo 'export SPOTIPY_CLIENT_ID='your-spotify-client-id'' >> ~/.bash_profile
    echo 'export SPOTIPY_CLIENT_SECRET='your-spotify-client-secret'' >> ~/.bash_profile
    echo 'export SPOTIPY_REDIRECT_URI='http://127.0.0.1/'' >> ~/.bash_profile
    ```
5. Create a new gmail account which will be used to send the emails out. 
    1. Once the account is created, you'll need grant access to less secure apps (Python).
    Head to https://myaccount.google.com/lesssecureapps and set
    "Allow less secure apps" to ON.
6. Add the newly created email user and password to environment variables:
    1. macOS 10.5 and higher:
    ```
    echo 'export EMAIL_USER='your-email-user@gmail.com'' >> ~/.zshenv
    echo 'export EMAIL_PASS='your-email-password'' >> ~/.zshenv
    ```
    2. Linux/bash:
    ```
    echo 'export EMAIL_USER='your-email-user@gmail.com'' >> ~/.bash_profile
    echo 'export EMAIL_PASS='your-email-password'' >> ~/.bash_profile
    ```
7. Get your Spotify user ID.
    1. Open Spotify.
    2. Click your username in the top right corner. 
    3. Towards the top middle of the screen, click the circle with 3 dots > share > 
    copy profile link. 
    4. Paste the link somewhere copy the name (in some cases digits) after /user/ 
    up until the `?` if you have one.
        1. Example looking at the link below: username = `marcspotify`
        ```
       https://open.spotify.com/user/marcspotify?si=lWvRnOr34duQKrPEUK1gFA
       ```
8. Open a new Terminal and run the following, where username is the name you got in 
the last step and `user@email.com` is delivery address. 
    ```
    python3 new_releases.py username user@email.com
    ```
   1. After running this the first time, you will need to authenticate. 
   Your web browser will open a blank page to http://127.0.0.1/ followed 
   by more text. copy the entire URL (including the http://127.0.0.1/) and 
   paste it back in the terminal. 
   2. The redirect for authentication is only needed one-time.  
   Running the same command again will only result in the email getting sent. 
