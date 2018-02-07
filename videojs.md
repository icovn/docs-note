https://github.com/videojs/video.js/wiki/Plugins

http://stevennick.github.io/videojs-ad-scheduler/



http://shaka-player-demo.appspot.com/docs/api/tutorial-welcome.html 



https://github.com/mrocajr/murphy - Live Stream error simulator



https://handbrake.fr/downloads.php - The open source video transcoder

\#\#\#build videojs

\#clone

git clone https://github.com/videojs/video.js.git

npm install -g grunt-cli

npm install -g contribflow

cd video.js

git remote add upstream https://github.com/videojs/video.js.git



\#update

git checkout master

git pull upstream master



\#install dependencies

npm install



\#build

grunt dist

grunt test

grunt karma:dev



\#\#Making Changes

\#start a new development branch

contrib feature start

\#sandbox

cp sandbox/index.html.example sandbox/index.html

open sandbox/index.html

\#testing locally

cp sandbox/index.html.example sandbox/index.html

grunt connect

open http://localhost:9999/sandbox/index.html

\#commit and push changes

git add .

git commit -av

git push 

\#submitting your changes

git push --set-upstream origin master

contrib feature submit

\#clean up

git checkout \(branchname\)

contrib feature delete

contrib hotfix delete

