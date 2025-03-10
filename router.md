Роутинг необходимая часть SPA приложений, т.к. при изменении URL мы не делаем запрос на сервер и тем самым не перезагружаем страницу.
Для реализации данного функционала можно так же найти готовые решения, но можно и написать самому т.к.
базовый функционал роутера не содержит в себе ничего необычного, логику работы роутера я бы разбил на следующие пункты:
  1. Регистрация страниц т.е. определяем эндпоинт и компонент который будет отображаться по данному эндпоинту.
  2. Изменение состояния браузера т.е. работа с history api и window.location.pathname
  3. Отображение нужной страницы после очистки корневой ноды

```javascript
  export class Router {
    routes = {} as Record<string, HTMLElement>;

    constructor(private root: HTMLElement) {}

    // Пункт 1
    public registerRoute(route: string, pageEl: HTMLElement) {
      this.routes[route] = pageEl;
    }

    // Пункт 2
    private goTo(str: string, addHistory = true) {
      if (addHistory) {
        history.pushState({ str }, "", str);
      }
      const template = this.routes[str];
      this.renderPage(template);
    }

    // Пункт 3
    private renderPage(template: Page["element"]) {
      this.root.innerHTML = "";
      this.root.append(template);
    }
  }
```