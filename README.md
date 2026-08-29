# Frequency Guessing Game

A small web game where you have to **listen to randomly generated frequencies and guess them using sliders**.

## How to Play

1. Choose a difficulty level.
2. Press **Play** to hear the generated frequencies.
3. Use the sliders to select the frequencies you think you heard.
4. Press **Check** to see your error percentage.
5. Try to get as close to **0% error** as possible.
6. Press **Reset** to start a new game.

## Difficulty Levels

The difficulty determines how many frequencies are played:

| Level | Frequencies   |
| ----- | ------------- |
| 1     | 1 frequency   |
| 2     | 2 frequencies |
| 3     | 3 frequencies |

The frequencies are randomly generated between **250 Hz and 800 Hz**.

## Features

* Frequency generation using the Web Audio API.
* Seeded random generation so the same challenge remains consistent until it is solved.
* 1–3 frequencies depending on the selected level.
* Interactive frequency sliders.
* Percentage-based error calculation.
* Mobile-friendly interface.
* Play and Stop controls.
* Reset functionality.

## Built With

* [Vue.js](https://vuejs.org/)
* [Vite](https://vite.dev/)
* JavaScript
* Web Audio API
* HTML & CSS

## Compatibility

The game is designed primarily for **mobile devices**, but it can also be played on desktop browsers.

## License

No license is currently specified for this project.

