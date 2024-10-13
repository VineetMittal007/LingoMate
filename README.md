# LingoMate

## Project setup

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

![App Screenshot](./LingoMate.png)
![App Screenshot](./Screenshot%202024-10-13%20173347.png)

## Features

- Translation History is stored in the localstorage. So even, If the user refreshes the page the Translation history remains there.
- Added the Dark/Light Mode.
- Also added a .txt feature to translate an entire file and download for such.
- 58 languages were used. You can find JSON for this in language.js in src/language.js.
- You can also swap the languages.



#### Further Improvements
- The Github button at the top right has one error. The entire button on clicking won't take you to the repository of the page. You have to click on the Github that is written.
- When the page is refreshed, it will automatically switch to light mode . Because I have not stored the previous state of the mode in any way.
- I wanted to add TexttoSpeech using SpeechSynthesis Api but it had a lot of bugs. It was not suitable for 55+languages. Sometimes it worked and sometimes it did not. So I dumped that idea.  Although, If I had a Better FREE Api, I would have added that as well.
- Translation history can have a download option as well.
- If the input is very large then the Translation will have a very large paragraphs. 
- And since we know, its stored in localStorage, this large file could cover a large part of your largestorage. You have to delete this particular file to clear up your localStorage.


### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


## Support

For support or suggestions, email vineetmittalxvnm@gmail.com  .

## Authors

- [@vineet_mittal](https://www.github.com/VineetMittal007)



