# Prepare directory structure
Because of how the Docker environment deals with file and folder permissions, we'll have to start with a good folder structure, or we'll eventually be dealing with a lot of unneeded I/O operations.

Your structure should look like the following.

```
data
├── torrents
│   ├── books
│   ├── movies
│   ├── music
│   └── tv
├── usenet
│   ├── incomplete
│   └── complete
│        ├── books
│        ├── movies
│        ├── music
│        └── tv
└── media
     ├── books
     ├── movies
     ├── music
     └── tv
```
Also, because of how Linux handles file and folder permissions, it's important to give the right users the right permissions for these folders and the files that they will contain. You can do this by running the following commands.

```bash
sudo chown -R $USER:$USER /data
sudo chmod -R a=,a+rX,u+w,g+w /data
```

If you would like more information about this, you can follow the following link: [TRaSH-Guides](https://trash-guides.info/Hardlinks/How-to-setup-for/Docker/)

# Usage
Make sure you have "docker" and "docker-compose" installed on your system.

1. Clone the repository
```bash
git clone https://github.com/<your-user>/lankrui-media-server-containers.git
```

2. Change to the cloned directory
```bash
cd lankrui-media-server-containers
```

3. Create a `.env` file inside the `servarr` folder from the example and fill in the required environment variables
(Note if you're using Plex, you have to acquire your own claim token from [Plex](https://www.plex.tv/claim/))
```bash
cp servarr/.env.example servarr/.env
```

4. Start the containers from inside the 'servarr' folder, and optionally also start your portainer instance from inside the 'portainer' folder
```bash
docker-compose up -d
```
After all containers have started, you can start configuring each service to your liking, good luck!

# Recyclarr
Quality profiles and custom formats are managed by [Recyclarr](https://recyclarr.dev) (v8 config format, TRaSH-Guides backed). `servarr/configurations/recyclarr/recyclarr.yml` is the source of truth; copy it to `/docker-configs/recyclarr/` on the server. API keys live in `secrets.yml` next to it (not in this repo):

```yaml
sonarr_url: http://172.17.0.1:8989
sonarr_apikey: <key>
sonarr_anime_url: http://172.17.0.1:8998
sonarr_anime_apikey: <key>
radarr_url: http://172.17.0.1:7878
radarr_apikey: <key>
```

After adding a `trash_id` profile that already exists in Sonarr/Radarr, run `recyclarr state repair --adopt` once so it is adopted instead of duplicated. Preview changes before applying with `recyclarr sync --preview`.
