
# Build the image (Uses the Dockerfile in this directory)
docker build -t superset-build .

# Secret key should be changed and kept secret, not published to GitHub :)
docker run -d -v ${PWD}:/data:rw -p 8080:8088 -e "SUPERSET_SECRET_KEY=your_new_secret_key" --name superset superset-build

# Explanation for the -v ${PWD}:/data:rw flag:
# This flag mounts the current working directory (${PWD}) on the host machine to the /data directory inside the container.
# You should replace ${PWD} with the absolute path to the folder where their data files will be stored.
# For example: -v /Users/yourname/superset_data:/data:rw
# Or if you are in the folder where the data files are located, you can use -v $(pwd):/data:rw
# The :rw option allows read and write access to the mounted directory.
# This ensures that the container has access to the necessary data files.

# Update user, firstname, lastname, email and password as you see fit
docker exec -it superset superset fab create-admin --username admin --firstname Admin --lastname Superset --email admin@example.com --password admin
docker exec -it superset superset db upgrade
docker exec -it superset superset init