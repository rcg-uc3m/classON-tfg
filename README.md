Instructions to use this version of classON
===========================================

## Installation

1. Clone que repo
`git clone https://github.com/gootyfer/classON-UAB.git`

2. Install software dependencies
    - Install a node.js version manager, [nvm](https://github.com/creationix/nvm#installation)
    - Install node.js v6.0 (newer versions cannot run the app) `nvm install 6.0`
    - [Install mongodb](https://docs.mongodb.com/manual/installation/)

3. Install project dependencies
`npm install` (from the project folder)

4. [Launch mongodb as a service](https://hevodata.com/blog/install-mongodb-on-ubuntu/)

    - Edit service configuration file: `sudo vim /etc/systemd/system/mongodb.service` 

    - Copy the following contents in the file.
        ```
        #Unit contains the dependencies to be satisfied before the service is started.
        [Unit]
        Description=MongoDB Database
        After=network.target
        Documentation=https://docs.mongodb.org/manual
        # Service tells systemd, how the service should be started.
        # Key `User` specifies that the server will run under the mongodb user and
        # `ExecStart` defines the startup command for MongoDB server.
        [Service]
        User=mongodb
        Group=mongodb
        ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf
        # Install tells systemd when the service should be automatically started.
        # `multi-user.target` means the server will be automatically started during boot.
        [Install]
        WantedBy=multi-user.target
        ```

    - Update the systemd service: `systemctl daemon-reload`
    - Start the service with systemcl: `sudo systemctl start mongodb`

    - Check if mongodb has been started on port 27017 with netstat command: `netstat -plntu `
    - Check if the service has started properly: `sudo systemctl status mongodb `
    - Enable auto start MongoDB when system starts: `sudo systemctl enable mongodb `
    - Stop MongoDB `sudo systemctl stop mongodb `
    - Restart MongoDB `sudo systemctl restart mongodb `

5. Start the server with node.js v6.0
`nvm run 6.0 index.js`

## Using classON (demo)

To access to the teacher version of the app, go to the browser at
http://localhost:3000/teacher/index.html?teacher=isra&session=p1
You must indicate as parameters of the url
- `teacher`: name of the teacher that is assigned to this course
- `session`: number of the session to manage 

  session can be checked in the student webpage souce code, it is passed to `classon.js` as a get parameter [`<script type="text/javascript" src="../js/classon.js?session=p1"></script>`] 
- `group` is assumed to be `uab` (for this teacher)

To access to an example of the student version go to
http://localhost:3000/student/p1/
You will be asked to log in with the student id (or ids if working in pairs) and the position in the classroom.

Before being able to log in (as a student), you must populate the database with users. For that, you may use the utility to import users from CSV at `users/saveUsersFromCSV`.
Usage: `node saveUsersFromCSV.js CSVfilename groupName`
- `CSVfilname` is the path to the CSV file with the users data
- `groupName` is the name give to these group of users (usually their class)

`users/students.csv` contains a sample of students data

The `position` in the student login form refers to their location in classroom (which computer they are working on). 
Current version expects a number between 1 and 12 (for http://localhost:3000/teacher/index.html?teacher=isra&session=p1). 

If you want to use classON with your own assignment, you can use the classON author tool but it's *still unfinished*:
http://localhost:3000/teacher/author/
