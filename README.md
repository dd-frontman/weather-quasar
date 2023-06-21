## Приложение для прогноза погоды на Vue.js с использованием Quasar Framework и препроцессор SASS.
![preview](src/assets/preview-img.png "weather-quasar")
**Основной компонент включает в себя:**

* Поле ввода для поиска города.
* Кнопку для получения текущего местоположения пользователя и отображения погоды в этом месте.
* При успешном поиске, отображается информация о погоде в выбранном городе.
* В скриптовой части, используется API OpenWeatherMap для получения данных о погоде. Сначала код получает местоположение пользователя или использует введенный в поле поиск город. Затем код отправляет запрос на сервер погоды и получает ответ, который включает в себя информацию о температуре, состоянии погоды и иконке погоды. Эти данные затем отображаются на странице.
* Стилизация страницы меняется в зависимости от времени суток (день/ночь), что определяется по иконке погоды. Стили создают градиентный фон и делают круглыми углы иконки погоды.

**[Ссылка на приложение](https://den-dev97.github.io/weather-quasar/dist/spa/#/ "weather-quasar")**

## Vue.js Weather Forecast Application using Quasar Framework. Styles are written in SASS preprocessor.

**The main component includes:**

* An input field for city search.
* A button for obtaining the current location of the user and displaying the weather at this location.
* Upon successful search, weather information in the selected city is displayed.
* In the script part, OpenWeatherMap API is used to obtain weather data. The code first gets the user's location or uses the city entered in the search field. Then, the code sends a request to the weather server and gets a response, which includes information about temperature, weather condition, and weather icon. These data are then displayed on the page.
* The page styling changes depending on the time of day (day/night), determined by the weather icon. The styles create a gradient background and round the corners of the weather icon.

**[Link on app](https://den-dev97.github.io/weather-quasar/dist/spa/#/ "weather-quasar")**

## Install the dependencies
```bash
yarn || npm i
```
### Start the app in development mode (hot-code reloading, error reporting, etc.)
```bash
quasar dev
```
### Lint the files
```bash
yarn run lint
```
### Build the app for production
```bash
quasar build
```
