# Setting up an Express/Sequelize app

These steps are for creating an `express` app which uses `sequelize` to interact with a `MySQL` database. The steps assume you have `MySQL` running in a `docker` container on your machine. It also references the [`music-api`](https://github.com/MCRcodes/music-library-api-mysql/tree/master). If you get stuck, it would be a good idea to check back there for examples. Use this as a bootstrap to set up your own app. 

Remember: To be considered complete, you api should be able to perform all the `CRUD` actions.

## Some simple database ideas
These ideas are intended to only require a single table. Feel free to try more complex ideas.

- A to-do api. Tasks are stored in a table, and have a status (todo, in-progress, done). 
- A quiz questoin database, each question has a query and an answer.
- A `top-trumps` style database. Each `card` has a name, flavour text and a set of stats.

## Repository Setup
- Create a new directory for the project on your computer
- Create a new `README.md` file in the root of you project
- Initialise a git repo in the new directory with `git init`
- Add an empty `.gitignore` file
- Create a repository for the project on Github
- Connect your locat repository to the remote GitHub repository
- Initialise a new `NPM` project in you project directory with `npm init`
- Add `node_modules` to your `.gitignore` file
- Install `eslint-config-mcr-codes` as a dev dependency, configure it according to these [instructions](https://www.npmjs.com/package/eslint-config-mcr-codes?activeTab=readme)

## Application Setup
- Install `express` and `sequelize` as dependencies
- Set up your basic `index.js` and `src/app.js` similar to how you did in the `Music API`
    - `src/app.js` should configure and export a basic Express application.
    - `index.js` should call `app.listen()` and then `console.log()` the port that the app is listening on.
- Install `dotenv` and `nodemon` as development dependencies
- Add a `start` script to your `package.json` file with the following command: `nodemon -r dotenv/config index.js`. This uses `nodemon` to execute the `index.js` file with the environment variables loaded from a `.env` file.
- Create a scripts folder, and copy the `create-database.js` and `drop-database.js` scripts from the `music-library`
- Add a `prestart` script to your `package.json`. Set the command to `node scripts/create-database.js`. This will run before the `npm start` command, and will create your database if it doesn't already exist.
- Create a `.env` file with the following variables:
    - DB_PASSWORD=PASSWORD
    - DB_NAME=APP_NAME
    - DB_USER=root
    - DB_HOST=localhost
    - DB_PORT=3306
- Add your `.env` to your `.gitignore`
- Create a folder at `src/models` and add an `index.js`:
    - Require `sequelize` at the top of this file
    - Declare a function as `setupDatabase`. Declare a `const` and assign a `new Sequelize()` to it. Pass it the connection infomation from your `.env`
    - Still inside the funciton, call `sequelize.sync({alter: true})`
    - Have the function return an empty object for now.
    - At the bottom of the file, `module.exports = setupDatabase()`
     

At this point you should be able to run `npm start` in your terminal, and get a listening express server.

## Test Environment Setup
- Install `mocha`, `chai`, and `supertest` as dev dependencies.
- Copy accross the `test-setup.js` from the `music-api`
- Add a `.env.test` with the same environment variables as your `.env`. Make sure to give your test database a different name.
- Add `.env.test` to your `.gitignore`
- Add a `test` script to your `package.json` file: `mocha tests/**/*.test.js --opts .mocha.opts`
- Add a `pretest` script to your `package.json`. Set the command to: `node scripts/create-database.js test`. Note that this time we pass the `test` option at the end of the command. This tells the script to load the variables from `.env.test` instead of `.env`.
- Add a `posttest` script, set the command to: `node scripts/drop-database.js`. This will delete your test database after your tests have finished running.

At this point, you should be able to run `npm test` in your terminal. Mocha should run and complain that there are not tests yet. Commit your work to github.
