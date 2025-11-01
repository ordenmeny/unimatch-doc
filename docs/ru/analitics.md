# Аналитика через Яндекс Метрика

### 1. Подключение базового счётчика Метрики

#### Вставить код счётчика в public/index.html, перед закрывающим тегом </head>. Этот код должен быть на всех страницах, поэтому для SPA достаточно разместить его один раз в index.html.

```javascript
<!-- Yandex.Metrika counter -->
<script type="text/javascript">
  (function(m,e,t,r,i,k,a){
      m[i]=m[i]||function(){(m[i].a=m[i].a||[]).push(arguments)};
      m[i].l=1*new Date();
      for (var j = 0; j < document.scripts.length; j++) {
        if (document.scripts[j].src === r) return;
      }
      k=e.createElement(t),a=e.getElementsByTagName(t)[0];
      k.async=1;k.src=r;a.parentNode.insertBefore(k,a);
  })(window, document, 'script', 'https://mc.yandex.ru/metrika/tag.js?id=105074228', 'ym');

  ym(105074228, 'init', {
      clickmap:true,
      trackLinks:true,
      accurateTrackBounce:true,
      webvisor:true,
      ssr:true,
      ecommerce:"dataLayer"
  });
</script>
<noscript>
  <div><img src="https://mc.yandex.ru/watch/105074228" style="position:absolute; left:-9999px;" alt=""/></div>
</noscript>
<!-- /Yandex.Metrika counter -->
```

### 2. Отслеживание переходов между страницами (SPA)
#### Поскольку React Router не перезагружает страницу, нужно вручную отправлять информацию о смене URL в Метрику.
#### Можно создать компонент YandexMetrikaTracker.jsx:
```javascript
import { useEffect } from "react";
import { useLocation } from "react-router-dom";

const METRIKA_ID = 105074228;

export default function YandexMetrikaTracker() {
  const location = useLocation();

  useEffect(() => {
    if (window.ym) {
      window.ym(METRIKA_ID, "hit", location.pathname + location.search);
      console.log("[YandexMetrika] Page view:", location.pathname);
    }
  }, [location]);

  return null;
}
```

#### Подключение (в App.js или в корневом компоненте, где используется BrowserRouter):
```javascript
import { BrowserRouter as Router } from "react-router-dom";
import YandexMetrikaTracker from "./YandexMetrikaTracker";
import Routes from "./Routes"; // маршруты

function App() {
  return (
    <Router>
      <YandexMetrikaTracker />
      <Routes />
    </Router>
  );
}

export default App;
```

### 4. Личный кабинет пользователя
#### Пример компонента:
```javascript
import { useEffect } from "react";

const METRIKA_ID = 105074228;

export default function Dashboard({ user }) {
  useEffect(() => {
    if (window.ym && user) {
      // Сообщаем Метрике, что пользователь вошёл в личный кабинет
      window.ym(METRIKA_ID, "reachGoal", "enter_dashboard", {
        user_id: user.user_id,
        user_name: user.user_name,
        first_name: user.first_name,
        last_name: user.last_name,
      });

      // Также можно задать userParams (для User ID)
      window.ym(METRIKA_ID, "userParams", {
        id: user.user_id,
        name: user.user_name,
        first_name: user.first_name,
        last_name: user.last_name,
      });

      console.log("[YandexMetrika] User entered dashboard:", user.user_id);
    }
  }, [user]);

  return (
    <div>
      <h1>Личный кабинет</h1>
      <p>Добро пожаловать, {user.first_name}!</p>
    </div>
  );
}
```
