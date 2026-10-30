Campus Library — GraphQL Tutorial 07

A small Campus Library web application built for CS543: Web Services. The project demonstrates how a plain HTML/CSS front end communicates with a GraphQL API built with Node.js and Apollo Server, which reads and writes data in a MySQL database.

Features

Lists authors and their books, including each book's title and publication year.

Uses a GraphQL query to retrieve authors and their related books.

Adds a new book through a form using a GraphQL mutation.

Stores authors and books in MySQL, linked through a foreign key.

Technology Stack

Frontend: HTML, CSS, JavaScript

API: Node.js, Apollo Server, GraphQL

Database: MySQL

Development tools: VS Code and Live Server

Project Structure

GraphQL_Tut_07/
├── Screenshot/
├── db.js
├── index.html
├── package.json
├── package-lock.json
├── schema.sql
├── server.js
├── style.css
└── README.md

Prerequisites

Install the following before running the project:

Node.js (v18 or newer)

MySQL Community Server

Visual Studio Code

The Live Server extension for VS Code

Check that Node.js and npm are installed:

node -v
npm -v

Setup Instructions

1. Install project dependencies

Open a terminal in the project directory and run:

npm install

If the dependencies have not been installed yet, the required packages are:

npm install @apollo/server graphql mysql2

2. Create the MySQL database

Log in to MySQL:

mysql -u root -p

Then run the SQL script from the project directory. From the MySQL prompt:

SOURCE schema.sql;

The script creates the gqldemo database, the authors and books tables, and sample records.

You can check the sample data with:

USE gqldemo;
SELECT * FROM authors;
SELECT * FROM books;

3. Configure the database connection

Open db.js and set the password to your own MySQL password:

const pool = mysql.createPool({
  host: 'localhost',
  user: 'root',
  password: 'YOUR_MYSQL_PASSWORD',
  database: 'gqldemo'
});

Replace YOUR_MYSQL_PASSWORD with your local MySQL password. Do not commit real passwords or other secrets to GitHub.

4. Start the GraphQL API

In the project directory, run:

node server.js

The API should be available at:

http://localhost:4000/

Keep this terminal running while using the web page.

5. Open the frontend

Open index.html in VS Code, right-click it, and select Open with Live Server. The page should open in your browser and request data from the GraphQL API.

If the page does not load data, confirm that the API is running on port 4000 and that the API URL in index.html is http://localhost:4000/.

GraphQL Operations

Query: list authors and their books

Run this query in the Apollo Sandbox at http://localhost:4000/:

query {
  authors {
    id
    name
    country
    books {
      title
      year
    }
  }
}

The response contains each author and the books associated with that author.

Mutation: add a book

The web page's Add a book form sends the addBook mutation. A corresponding GraphQL operation is:

mutation {
  addBook(title: "GraphQL for Beginners", year: 2025, authorId: "2") {
    id
    title
    year
  }
}

The mutation inserts the book into MySQL. The page then reloads the list so the new book can appear under its author.

Database Relationships

One author can have multiple books.

Each book references an author through author_id.

The foreign key in the books table links each book to the corresponding row in authors.

Troubleshooting

Database connection error: Check that MySQL is running and that the username, password, and database name in db.js are correct.

Unknown database gqldemo: Run SOURCE schema.sql; from the MySQL prompt.

Page remains on “Loading…”: Start the API with node server.js and verify that index.html uses http://localhost:4000/.

Dependencies missing: Run npm install in the project directory.

Book is not added: Check the browser console and the terminal running the GraphQL API for errors.

Learning Outcomes

This project demonstrates:

Creating a relational database with MySQL.

Defining a GraphQL schema with types, queries, and mutations.

Writing resolvers that retrieve and insert data using SQL.

Fetching nested related data in a single GraphQL request.

Connecting a simple HTML/CSS/JavaScript frontend to a GraphQL API.

Course: CS543 — Web Services
Tutorial: 07 — A First Hands-On GraphQL Tutorial
