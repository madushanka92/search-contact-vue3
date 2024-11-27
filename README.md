# Contact Search App
This is a simple Vue 3 application for searching and managing contact information. It allows users to input contact details, including first name, last name, date of birth, address, phone number, email, and postal code. It also features basic validation for postal code and phone number formats.

# Prerequisites
Node.js: Make sure you have Node.js installed on your machine. If not, download and install the latest LTS version.
npm: This is usually installed alongside Node.js. You can check by running npm --version in your terminal.

# Getting Started
Follow these steps to get the project up and running:

### 1.  Clone the repository

First, clone the repository to your local machine:
```
git clone https://<repository-url>.git
cd contact-search
```

### 2. Install dependencies
Once inside the project folder, run the following command to install all required dependencies:
```
npm install
```
This will install all dependencies listed in the package.json file, including Vue, Bootstrap, and other necessary packages.

### 3. Run the development server
To start the development server and run the app locally, use the following command:

``` 
npm run dev
```
The app will be available at http://localhost:5173 in your browser.

### 4. Build for production
When you're ready to deploy the app, you can build the project for production using the following command:

```
npm run build
```
This will generate a dist/ folder containing the optimized production-ready files.

### 5. Preview the production build
To preview the production build locally, you can run:
```
npm run preview
```
This will start a local server that serves the production build of your app.

### Available Scripts
In the project directory, you can run the following commands:
1. ```npm run dev``` - Starts the development server and watches for changes.
2. ```npm run build``` - Builds the app for production.
3. ```npm run preview``` - Previews the production build locally.

### Folder Structure
Here's an overview of the important folders and files in the project:
```
/contact-search
├── /node_modules              # Installed dependencies
├── /public                    # Public assets
│   └── index.html             # Main HTML file
├── /src
│   ├── /assets                # Static assets (images, fonts, etc.)
│   ├── /components            # Vue components
│   ├── /views                 # Vue views
│   ├── App.vue                # Root component
│   ├── main.js                # Main entry point for the app
├── package.json               # Project metadata and dependencies
├── vite.config.js             # Vite configuration file
└── README.md  
```

### Using Bootstrap
This app uses Bootstrap 5 for styling. The required Bootstrap CSS and JavaScript files are included in the project dependencies.
To use Bootstrap's grid system, components, and utilities, you can refer to the Bootstrap 5 documentation.

### Vue 3 Features Used
This app uses the following Vue 3 features:

* Composition API: Used for state management, lifecycle hooks, and reactivity.
* ```defineProps``` & ```defineEmits```: Used for prop and event handling in components.
* ```reactive```: Used for reactive state management within the component.
* ```watch```: Used to react to changes in the component’s props or state.

### Styling
This app uses Sass for styling. ```npm install``` will install the necessary Sass packages to use .scss files. The styling is based on Bootstrap 5's utilities, and custom styles are defined in the style section of Vue components.

### Troubleshooting
If you encounter issues with the development server, try stopping it and running npm install again to ensure all dependencies are correctly installed.
For production builds, if you're experiencing issues with missing assets or incorrect paths, check the vite.config.js for configuration details.