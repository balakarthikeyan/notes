FROM node:18.13.0

# RUN apt-get install -y python make gcc g++ 

# Install Google Chrome Stable and fonts, libs to make the browser work with Puppeteer.
RUN apt-get update && apt-get install gnupg wget -y && \
    wget --quiet --output-document=- https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor > /etc/apt/trusted.gpg.d/google-archive.gpg && \
    sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' && \
    apt-get update && \
    apt-get install google-chrome-stable -y --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Create a user with name 'app' and group that will be used to run the app
RUN groupadd -r app && useradd -rm -g app -G audio,video app

# Create a working directory inside the container
WORKDIR /home/app

# Copy package.json and package-lock.json to the working directory
COPY package*.json /home/app/

# Install application dependencies
RUN npm install

# Copy the rest of your application source code to the working directory
COPY . /home/app

# Build your application
RUN npm run build

# Set the working directory to the ‘dist’ folder
WORKDIR /home/app/dist

# Expose a port (if your application listens on a specific port)
EXPOSE 4000

# Give app user access to all the project folder
RUN chown -R app:app /home/app
RUN chmod -R 777 /home/app

USER app

# Define the command to run your application
CMD [“node”, “server.js”]


# server.js
````
const browser = await puppeteer.launch({
  executablePath: '/usr/bin/google-chrome',
  headless: 'new',
  ignoreDefaultArgs: ['--disable-extensions'],
  args: ['--no-sandbox', '--disable-setuid-sandbox'],
});
````