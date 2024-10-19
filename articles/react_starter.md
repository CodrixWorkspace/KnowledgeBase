## Introduction

Welcome to this beginner-friendly guide on creating and building a React application on a Windows machine using Visual Studio Code (VS Code). React is a popular JavaScript library for building user interfaces, particularly single-page applications. Developed and maintained by Facebook, React allows developers to create large web applications that can update and render efficiently in response to data changes.

React is widely used in modern web development due to its component-based architecture, which promotes reusability and scalability. Companies like Facebook, Instagram, Netflix, and Airbnb utilize React in their front-end development.

In this guide, we'll walk you through the step-by-step process of setting up your development environment, creating a new React application, and running it locally. We'll also introduce key concepts that are essential for effectively working with React.

## Prerequisites

Before we begin, ensure you have the following:

- A Windows PC with administrative privileges
- An internet connection
- Basic knowledge of JavaScript and HTML/CSS (helpful but not mandatory)

## Setting Up the Development Environment

### Install Node.js

Node.js is required to run JavaScript outside the browser and comes with npm (Node Package Manager), which we'll use to install React.

<B>Steps:</B>
    
- Go to the [Node.js official website](https://nodejs.org/en/download/package-manager) and download the LTS (Long Term Support) version for Windows.
- Run the installer and follow the prompts.
- To verify the installation, open Command Prompt and run:
```
    node -v
    npm -v
```
You should see the versions of Node.js and npm displayed.

### Install Visual Studio Code

VS Code is a lightweight but powerful source code editor.

<B>Steps:</B>

- Download VS Code from the [official website](https://code.visualstudio.com/Download).
- Run the installer and follow the installation prompts.

### Install Git (Optional but Recommended)

Git is a version control system that helps manage your codebase.

<B>Steps:</B>

- Download Git from the [official website](https://git-scm.com/downloads/win).
- Run the installer and follow the prompts.

## Creating a New React Application

We'll use <B>Create React App</B>, a comfortable environment for learning React and building a new single-page application.

- Open Command Prompt and Navigate to your Preferred Directory
- Run `npx create-react-app simple-react-app` to create a new react app. `npx` comes with npm 5.2+ and runs packages without installing them globally
- Enter `cd simple-react-app` to navigate to the app directory
- Enter `code .` to open the project in VS Code

If this doesn't work, you may need to install the 'code' command in your PATH:

- In VS Code, press Ctrl + Shift + P.
- Type shell command and select Shell Command: Install 'code' command in PATH.

## Running the React Application

In the VS Code terminal (or Command Prompt), start the development server:

```
npm start
```

This runs the app in development mode. Open http://localhost:3000 to view it in your browser.

## Exploring the Project Structure

Your project contains the following important directories and files:

- public/: Static files like index.html.
- src/: Source code directory.
    - index.js: JavaScript entry point.
    - App.js: Main component.
- package.json: Lists dependencies and scripts.


## Key Concepts in React

Understanding the following key concepts will help you work effectively with React:

1. <B>Components</B>

Components are the building blocks of a React application.

    - Functional Components: JavaScript functions that return JSX.
    - Class Components: ES6 classes that extend React.Component.

Learn more: [React Components](https://19.react.dev/learn/describing-the-ui)

2. <B>JSX (JavaScript XML)</B>

JSX is a syntax extension that allows mixing HTML with JavaScript.

<B><I>Example:</I></B>
```
function HelloReact() {
  return <h1>Hello, React!</h1>;
}
```
Learn more: [Writing Markup with JSX](https://18.react.dev/learn/writing-markup-with-jsx)

3. <B>Props and State</B>
- Props: Short for properties; they are read-only inputs to components.
- State: Represents mutable data that affects what is rendered.

Learn more: [State and Lifecycle](https://18.react.dev/learn/managing-state)

4. <B>Hooks</B>

Hooks let you use state and other React features without writing a class.

- <B><I>useState:</I></B> Allows you to add React state to functional components.
- <B><I>useEffect:</I></B> Lets you perform side effects in functional components.

Learn more: [Introducing Hooks](https://18.react.dev/reference/react/hooks)