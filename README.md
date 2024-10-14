# LingoMate

LingoMate is a user-friendly translation application built with Vue.js. It allows users to translate text in real-time across a wide range of languages, all while offering an intuitive interface. With LingoMate, you can effortlessly convert text from one language to another and manage your translation history seamlessly.

## Technologies Used

- Vue.js: The front-end framework used for building interactive user interfaces.
- CSS3: For styling the web application.
- Local Storage: Used to save translation history persistently across sessions.

## Project setup

To get started with LingoMate, follow the setup instructions below. Once installed, you can run the application locally and start translating your text seamlessly.

### 1. Clone the Repository

```bash
git clone https://github.com/VineetMittal007/LingoMate.git

```
### 2. Navigate to the Project Directory
```
cd LingoMate
```

### 3.  Install Dependencies
Run the following command to install all the necessary dependencies:
```
yarn install
```
or 
```
npm install
```
### 4. Run the Development Server
To start the Vue.js development server locally:
```
yarn run
```
or 
```
npm run serve
```
After this, open http://localhost:8080 in your browser to see the project running.


## Screenshots

![App Screenshot](./Screenshot%202024-10-13%20234443.png)
![App Screenshot](./Screenshot%202024-10-13%20234505.png)
![App Screenshot](./Screenshot%202024-10-13%20234525.png)
![App Screenshot](./Screenshot%202024-10-13%20234538.png)

## Deployed 
LingoMate is deployed on GitHub Pages. You can view the live version at https://vineetmittal007.github.io/LingoMate/

## Features

- Choose from a wide range of languages for both input and output, including options like English, Hindi, Spanish, French, and many more.
-  A clean and responsive design ensures that users can easily navigate through the application.
- Keep track of previous translations with a history log that allows users to view and delete past entries.
- Translation History is stored in the localstorage. So even, If the user refreshes the page the Translation history remains there.
- The application offers a toggle between light and dark themes, providing a comfortable reading experience in different lighting conditions.
- Users can upload text files for translation, making it convenient for documents and larger texts.
- 58 languages were used. You can find JSON for this in language.js in src/language.js.
- You can also swap the languages.



#### Further Improvements
- The Github button at the top right has one error. The entire button on clicking won't take you to the repository of the page. You have to click on the Github that is written.
- When the page is refreshed, it will automatically switch to light mode . Because I have not stored the previous state of the mode in any way.
- I wanted to add TexttoSpeech using SpeechSynthesis Api but it had a lot of bugs. It was not suitable for 55+languages. Sometimes it worked and sometimes it did not. So I dumped that idea.  Although, If I had a Better FREE Api, I would have added that as well.
- Translation history can have a download option as well.
- If the input is very large then the Translation will have a very large paragraphs. 
- And since we know, its stored in localStorage, this large file could cover a large part of your largestorage. You have to delete this particular file to clear up your localStorage.
- Since the translation feature relies on online APIs, users must have a stable internet connection to use the app effectively. This could hinder usability in areas with poor connectivity.
- Currently, the app does not provide offline capabilities. Users cannot access translation features without an internet connection, which could be a significant limitation for mobile users.

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


## Support
If you'd like to contribute to LingoMate, feel free to fork the repository and submit a pull request. For any issues or suggestions, you can reach out via email or GitHub. Email vineetmittalxvnm@gmail.com  .

## Authors

- [@vineet_mittal](https://www.github.com/VineetMittal007)



